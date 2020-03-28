# Chapter 4: Learning

How do we learn to read, do math, and play sports? Learning in a neural network amounts to the **modification of synaptic weights**, in response to the local activity patterns of the sending and receiving neurons. As emphasized in previous chapters, these synaptic weights are what determine what an individual neuron detects, and thus are *the* critical parameters for determining neuron and network behavior.

In other words, *everything you know is encoded in the patterns of your synaptic weights*, and these have been shaped by every experience you've had (as long as those experiences got your neurons sufficiently active). Many of those experiences don't leave a very strong mark, and in much of the brain, traces of individual experiences are all blended together, so it is difficult to remember them distinctly (we'll see in the Memory Chapter that this blending can be quite beneficial for overall intelligence, actually). But each experience nevertheless drives some level of learning, and our big challenge in this chapter is to figure out how the mere influences of patterns of activity among individual neurons can add up to enable us to learn big things.

Biologically, **synaptic plasticity** (the modification of synaptic weights through learning) has been extensively studied, and we now know a tremendous amount about the detailed chemical processes that take place as a result of neural activity. We'll provide multiple levels of detail here (including a discussion of **spike timing dependent plasticity (STDP)**, which has captured the imaginations of many researchers in this area), but the high-level story is fairly straightforward: the overall level of neural activity on both ends of the synapse (sending and receiving neural firing) drives the influx of calcium ions (Ca++) via NMDA channels, and synaptic weight changes are driven by the **level of postsynaptic Ca++** in the *dendritic spine* associated with a given synapse. Low levels of Ca++ cause synapses to get weaker, and higher levels cause them to get stronger.

Computationally, many different sets of equations have been developed that can drive synaptic weight changes to accomplish many different computational goals. Which of these correspond to what the biology is actually doing? That is the big question. While a definitive answer remains elusive, we nevertheless have a reasonable candidate that aligns well with the biological data, and also performs computationally very useful forms of learning, which can solve the most challenging of cognitive tasks (e.g., learning to read or recognize objects).

There are two primary types of learning:

* **Self-organizing** learning, which extracts longer time-scale statistics about the environment, and can thus be useful for developing an effective **internal model** of the outside world (i.e., what kinds of things tend to reliably happen in the world -- we call these **statistical regularities**).
    
* **Error-driven** learning, which uses more **rapid contrasts between expectations and outcomes** to correct these expectations, and thus form more detailed, specific knowledge about contingencies in the world. For example, young children seem endlessly fascinated learning about what happens when they push stuff off their high chair trays: will it still fall to the ground and make a huge mess *this* time? Once they develop a sufficiently accurate expectation about exactly what will happen, it starts to get a bit less interesting, and other more unpredictable things start to capture their interest. As we can see in this example, error-driven learning is likely intimately tied up with curiosity, surprise, and other such motivational factors. For this reason, we hypothesize that neuromodulators such as **dopamine**, **norepinephrine** and **acetylcholine** likely play an important role in modulating this form of learning, as they have been implicated in different versions of surprise, that is, when there is a discrepancy between expectations and outcomes.

Interestingly, the main computational difference between these two forms of learning has to do with the time scale over which one of the critical variables is updated -- self-organizing learning involves averaging over a long time scale, whereas error-driven learning is much quicker. This difference is emphasized in the above descriptions as well, and provides an important source of intuition about the differences between these types of learning. Self-organizing learning is what happens when you blur your eyes and just take stuff in over a period of time, whereas error-driven learning requires much more alert and rapid forms of neural activity. In the framework that we will use in the rest of the book, we combine these types of learning into a single set of learning equations, to explore how we come to perceive, remember, read, and plan.

## Biology of Synaptic Plasticity

Learning amounts to changing the overall synaptic efficacy of the synapse connecting two neurons. The synapse has a lot of moving parts (see Chapter 2 Appendix on Neuron Biology), any one of which could potentially be the critical factor in causing its overall efficacy to change. How many can you think of? The search for the critical factor(s) dominated the early phase of research on synaptic plasticity, and evidence for the involvement of a range of different factors has been found over the years, from the amount of presynaptic neurotransmitter released, to number and efficacy of postsynaptic AMPA receptors, and even more subtle things such as the alignment of pre and postsynaptic components, and more dramatic changes such as the cloning of multiple synapses. However, the dominant factor for long-lasting learning changes appears to be the number and efficacy of postsynaptic AMPA receptors.

shows the five critical steps in the cascade of events that drives change in AMPA receptor efficacy. The **NMDA** receptors and the calcium ion (**Ca++**) play a central role -- NMDA channels allow Ca++ to enter the postsynaptic spine. Across all cells in the body, Ca++ typically plays an important role in regulating cellular function, and in the neuron, it is capable of setting off a series of chemical reactions that ends up controlling how many AMPA receptors are functional in the synapse. For details on these reactions, see Chapter Appendix on *Detailed Biology of Learning*. Here's what it takes for the Ca++ to get into the postsynaptic cell:

**1.** The postsynaptic membrane potential (Vm) must be elevated, as a result of all the excitatory synaptic inputs coming into the cell. The most important contributor to this Vm level is actually the **backpropagating action potential** -- when a neuron fires an action potential, it not only goes forward out the axon, but also backward down the dendrites (via active voltage-sensitive Na+ channels along the dendrites). Thus, the entire neuron gets to know when it fires -- we'll see that this is incredibly useful computationally.

**2.** The elevated Vm causes magnesium ions (Mg+) to be repelled (positive charges repel each other) out of the openings of NMDA channels, unblocking them.

**3.** The presynaptic neuron fires an action potential, releasing glutamate neurotransmitter into the synaptic cleft.

**4.** Glutamate binds to the NMDA receptor, opening it to allow Ca++ ions to flow into the postsynaptic cell. This only occurs if the NMDA is also unblocked. This dependence of NMDA on both pre and postsynaptic activity was one of the early important clues to the nature of learning, as we see later.

**5.** The concentration of Ca++ in the postsynaptic spine drives those complex chemical reactions that end up changing the number and efficacy of AMPA receptors. Because these AMPA receptors provide the primary excitatory input drive on the neuron, changing them changes the net excitatory effect of a presynaptic action potential on the postsynaptic neuron. This is what is meant by changing the synaptic efficacy, or *weight*.

Ca++ can also enter the postsynaptic cell via **voltage gated calcium channels (VGCC)'s** which are calcium channels that only open when the membrane potential is elevated. Unlike NMDA, however, they are *not* sensitive to presynaptic neural activity -- they only depend on postsynaptic activity. This has important computational implications, as we discuss later. VGCC's contribute less to Ca++ levels than NMDA, so NMDA is still the dominant player.

**Metabotropic glutamate receptors (mGlu)** also play an important role in synaptic plasticity. These receptors do not allow ions to flow across the membrane (i.e., they are not *ionotropic*), and instead they directly trigger chemical reactions when neurotransmitter binds to them. These chemical reactions can then modulate the changes in AMPA receptors triggered by Ca++.

increase, LTD {{=}} decrease) as a function of Ca++ concentration in the postsynaptic spine (accumulated over several 100 milliseconds). Low levels of Ca++ cause LTD, while higher levels drive LTP. Threshold levels indicated by theta values represent levels where the function changes sign.}}

We have been talking about changes in AMPA receptor efficacy without specifying which direction they change. **Long Term Potentiation (LTP)** is the biological term for long-lasting *increases* in AMPA efficacy, and **Long Term Depression (LTD)** means long-lasting *decreases* in AMPA efficacy. For a long time, researchers focused mainly on LTP (which is generally easier to induce), but eventually they realized that both directions of synaptic plasticity are equally important for learning. shows how this direction of change depends on the overall level of Ca++ in the postsynaptic spine (accumulated over a few 100's of milliseconds at least -- the relevant time constants for effects of Ca++ on synaptic plasticity are fairly slow) -- low levels drive LTD, while high levels produce LTP. This property will be critical for our computational model. Note that the delay in synaptic plasticity effects based on Ca++ levels means that the synapse doesn't always have to do LTD on its way up to LTP -- there is time for the Ca++ to reach a high level to drive LTP before the weights start to change.

### Hebbian Learning and NMDA Channels

The famous Canadian psychologist Donald O. Hebb predicted the nature of the NMDA channel many years in advance of its discovery, just by thinking about how learning should work at a functional level. Here is a key quote:

> Let us assume that the persistence or repetition of a reverberatory activity (or "trace") tends to induce lasting cellular changes that add to its stability.… When an axon of cell A is near enough to excite a cell B and repeatedly or persistently takes part in firing it, some growth process or metabolic change takes place in one or both cells such that A's efficiency, as one of the cells firing B, is increased.

This can be more concisely summarized as *cells that fire together, wire together.* The NMDA channel is essential for this process, because it requires both pre and postsynaptic activity to allow Ca++ to enter and drive learning. It can detect the *coincidence* of neural firing. Interestingly, Hebb is reputed to have said something to the effect of "big deal, I knew it had to be that way already" when someone told him that his learning principle had been discovered in the form of the NMDA receptor.

Mathematically, we can summarize Hebbian learning as:

$$ \Delta w = x y $$

where $\Delta w$ is the change in synaptic weight *w*, as a function of sending activity *x* and receiving activity *y*.

Anytime you see this kind of pre-post product in a learning rule, it tends to be described as a form of Hebbian learning. For a more detailed treatment of Hebbian learning and various popular variants of it, see the Hebbian Learning Apendix.

As we'll elaborate below, this most basic form of Hebbian learning is very limited, because weights will only go up (given that neural activities are rates of spiking and thus only positive quantities), and will do so without bound. Interestingly, Hebb himself only seemed to have contemplated LTP, not LTD, so perhaps this is fitting. But it won't do anything useful in a computational model. Before we get to the computational side of things, we cover one more important result in the biology.

## Spike Timing Dependent Plasticity

shows the results from an experiment by Bi and Poo in 1998 that captured the imagination of many a scientist, and has resulted in extensive computational modeling work. This experiment showed that the precise order of firing between a pre and postsynaptic neuron determined the sign of synaptic plasticity, with LTP resulting when the presynaptic neuron fired before the postsynaptic one, while LTD resulted otherwise. This **spike timing dependent plasticity (STDP)** was so exciting because it fits with the *causal* role of the presynaptic neuron in driving the postsynaptic one. If a given pre neuron actually played a role in driving the post neuron to fire, then it will necessarily have to have fired in advance of it, and according to the STDP results, its weights will increase in strength. Meanwhile, pre neurons that have no causal role in firing the postsynaptic cell will have their weights decreased. However, as we discuss in more detail in the Chapter Appendix on *STDP*, this STDP pattern does not generalize well to realistic spike trains, where neurons are constantly firing and interacting with each other over 100's of milliseconds. Nevertheless, the STDP data does provide a useful stringent test for computational models of synaptic plasticity. We base our learning equations on a detailed model using more basic, biologically-grounded synaptic plasticity mechanisms that does capture these STDP findings [@UrakuboHondaFroemkeEtAl08], but which nevertheless result in quite simple learning equations when considered at the level of firing rate.

## The eXtended Contrastive Attractor Learning (XCAL) Model

The learning function we adopt for the models in the rest of this text is called the **eXtended Contrastive Attractor Learning (XCAL)** rule. (The basis for this naming will become clear later). This learning function was derived through a convergence of bottom-up (motivated by detailed biological considerations) and top-down (motivated by computational desiderata) approaches. In the bottom-up derivation, we extracted an empirical learning function (called the **XCAL dWt function**) from a highly biologically detailed computational model of the known synaptic plasticity mechanisms, by [@UrakuboHondaFroemkeEtAl08] (see Chapter Appendix on *Detailed Biology of Learning* for more details). Their model builds in detailed chemical rate parameters and diffusion constants, etc, based on empirical measurements, for all of the major biological processes involved in synaptic plasticity. We capture much of the incredible complexity of the model (and by extension, hopefully, the complexity of the actual synaptic plasticity mechanisms in the brain) using a simple piecewise-linear function, shown below, that emerges from it. This XCAL dWt function closely resembles the function shown in , plotting the dependence of synaptic plasticity on Ca++ levels. It also closely resembles the (**BCM**) learning function.

The top-down approach leverages the key idea behind the BCM learning function, which is the use of a **floating threshold** for determining the amount of activity needed to elicit LTP vs LTD (see ). Specifically, the threshold is not fixed at a particular value, but instead adjusts as a function of average activity levels of the postsynaptic neuron in question over a long time frame, resulting in a **homeostatic** dynamic. Neurons that have been relatively inactive can more easily increase their synaptic weights at lower activity levels, and can thus "get back in the game". Conversely, neurons that have been relatively overactive are more likely to decrease their synaptic weights, and "stop hogging everything".

As we'll see below, this function contributes to useful **self-organizing** learning, where different neurons come to extract distinct aspects of statistical structure in a given environment. But purely self-organizing mechanisms are strongly limited in what they can learn -- they are driven by statistical generalities (e.g., animals tend to have four legs), and are incapable of adapting more pragmatically to the functional demands that the organism faces. For example, some objects are more important to recognize than others (e.g., friends and foes are important, random plants or pieces of trash or debris, not so much).

To achieve these more pragmatic goals, we need **error-driven** learning, where learning is focused specifically on correcting errors, not just categorizing statistical patterns. Fortunately, we can use the same floating threshold mechanism to achieve error-driven learning within the same overall mathematical framework, by adapting the threshold on a faster time scale. In this case, weights are increased if activity states are greater than their very recent levels, and conversely, weights decrease if the activity levels go down relative to prior states. Thus, we can think of the recent activity levels (the threshold) as reflecting **expectations** which are subsequently compared to actual **outcomes**, with the difference (or "error") driving learning. Because both forms of learning (self-organizing and error-driven) are quite useful, and use the exact same mathematical framework, we integrate them both into a single set of equations with two thresholds reflecting integrated activity levels across different time scales (recent and long-term average).

Next, we describe the XCAL dWt function (dWt = change in weight), before describing how it captures both forms of learning, followed by their integration into a single unified framework (including the promised explanation for its name!).

### The XCAL dWt Function

model, by fitting a piecewise linear function to the synaptic weight change behavior that emerges from it as a function of a wide range of sending and receiving spiking patterns." &gt;}}

The XCAL dWt function extracted from the [@UrakuboHondaFroemkeEtAl08] model is shown in . First, the main input into this function is the **total synaptic activity** reflecting the firing rate and duration of activity of the sending and receiving neurons. In mathematical terms for a rate-code model with sending activity rate x and receiving activity rate y, this would just be the "Hebbian" product we described above:
$$ \Delta w = f_{xcal} \left( x y, \theta_p\right) $$

where $f_{xcal}$ is the piecewise linear function shown in . The weight change also depends on an additional dynamic threshold parameter $\theta_p$, which determines the point at which it crosses over from negative to positive weight changes -- i.e., the point at which weight changes reverse sign. For completeness, here is the mathematical expression of this function, but you only need to understand its shape as shown in the figure:
$$
f_{xcal}(xy, \theta_p) = 
\begin{aligned}
(xy - \theta_p) & \mbox{if} \; xy > \theta_p \theta_d \\
-xy (1 - \theta_d) / \theta_d & \mbox{otherwise}
\end{aligned}
$$
where $\theta_d = .1$ is a constant that determines the point where the function reverses direction (i.e., back toward zero within the weight decrease regime) -- this reversal point occurs at $\theta_p \theta_d$, so that it adapts according to the dynamic $\theta_p$ value.

As noted in the previous section, the dependence of the NMDA channel on activity of both sending and receiving neurons can be summarized with this simple Hebbian product, and the level of intracellular Ca++ is likely to reflect this value. Thus, the XCAL dWt function makes very good sense in these terms: it reflects the qualitative nature of weight changes as a function of Ca++ that has been established from empirical studies and postulated by other theoretical models for a long time. The Urakubo model simulates detailed effects of pre/postsynaptic spike timing on Ca++ levels and associated LTP/LTD, but what emerges from these effects at the level of firing rates is this much simpler fundamental function.

As a learning function, this basic XCAL dWt function has some advantages over a plain Hebbian function, while sharing its basic nature due to the "pre * post" term at its core. For example, because of the shape of the dWt function, weights will go down as well as up, whereas the Hebbian function only causes weights to increase. But it still has the problem that weights will increase without bound (as long as activity levels are often greater than the threshold). We'll see in the next section that some other top-down computationally-motivated modifications can result in a more powerful form of learning while maintaining this basic form.

## Self-Organizing Learning: Long Time Scales and the BCM Model

The major computational motivation comes from a line of learning functions that began with , with these initials giving rise to the name of the function: **BCM**. (Interestingly Leon Cooper, a Nobel Laurate in Physics, was also "central" in the BCS theory of superconductivity). The BCM function is a modified form of Hebbian learning, which includes an interesting **homeostatic** mechanism that keeps individual neurons from firing too much or too little over time:
$$ \Delta w = x y \left(y-\theta\right) $$

where again x = sending activity, y = receiving activity, and $$\theta$$ is a **floating threshold** reflecting a **long time average** of the receiving neuron's activity:
$$ \theta = \langle y^2 \rangle $$
where $\langle \rangle$ indicates the expected value or average, in this case of the square of the receiving neuron's activation. shows what this function looks like overall -- a shape that should be becoming rather familiar. Indeed, the fact that the BCM learning function anticipated the qualitative nature of synaptic plasticity as a function of Ca++ () is an amazing instance of theoretical prescience. Furthermore, BCM researchers have shown that it does a good job of accounting for various behavioral learning phenomena, providing a better fit than a comparable Hebbian learning mechanism [@CooperIntratorBlaisEtAl04].

$$ \langle y \rangle_l $$

drives homeostatic behavior. Neurons that have low average activity are much more likely to increase their weights because the threshold is low, while those that have high average activity are much more likely to decrease their weights because the threshold is high.}}

BCM has typically been applied in simple feedforward networks in which, given an input pattern, there is only one activation value for each neuron. But how should weights be updated in a more realistic bidirectionally connected system with attractor dynamics in which activity states continuously evolve through time? We confront this issue in the XCAL version of the BCM equations:
$$ \Delta w = f_{xcal}( xy, \langle y \rangle_l) = f_{xcal}( xy, y_l) $$

where *xy* is understood to be the **short-term average synaptic activity** (on a time scale of a few hundred milliseconds -- the time scale of Ca++ accumulation that drives synaptic plasticity), which could be more formally expressed as: $\langle xy \rangle_s$, and $y_l = \langle y \rangle_l$ is the **long-term average activity of the postsynaptic neuron** (i.e., essentially the same as in BCM, but without the squaring), which plays the role of the $\theta_p$ floating threshold value in the XCAL function.

After considerable experimentation, we have found the following way of computing the $y_l$ floating threshold to provide the best ability to control the threshold and achieve the best overall learning dynamics:
$$
\begin{aligned}
\mbox{if} y > & .2 \mbox{then} \;  y_l = y_l + \frac{1}{\tau_l} \left( \mbox{max} - y_l \right) \\
\mbox{else} y_l = & y_l + \frac{1}{\tau_l} \left( \mbox{min} - y_l \right)
\end{aligned}
$$

This produces a well-controlled exponential approach to either the *max* or *min* extremes depending on whether the receiving unit activity exceeds the basic activity threshold of .2. The time constant for integration $$\tau_l$$ is 10 by default -- integrating over around 10 trials. See Chapter Appendix *XCAL_Details* for more discussion.

shows the main qualitative behavior of this learning mechanism: when the long term average activity of the receiver is low, the threshold moves down, and thus it is more likely that the short term synaptic activity value will fall into the positive weight change territory. This will tend to increase synaptic weights overall, and thus make the neuron more likely to get active in the future, achieving the homeostatic objective. Conversely, when the long term average activity of the receiver is high, the threshold is also high, and thus the short term synaptic activity is more likely to drive weight decreases than increases. This will take these over-active neurons down a notch or two, so they don't end up dominating the activity of the network.

### Self-organizing Learning Dynamics

This ability to spread the neural activity around in a more equitable fashion turns out to be critical for self-organizing learning, because it enables neurons to more efficiently and effectively cover the space of things to represent. To see why, here are the critical elements of the self-organizing learning dynamic (see subsequent simulation exploration to really get a feel for how this all works in practice):

* **Inhibitory competition** -- only the most strongly driven neurons get over the inhibitory threshold, and can get active. These are the ones whose current synaptic weights best fit ("detect") the current input pattern.
    
* **Rich get richer** positive feedback loop -- due to the nature of the learning function, only those neurons that actually get active are capable of learning (when receiver activity y = 0, then xy = 0 too, and the XCAL dWt function is 0 at 0). Thus, the neurons that already detect the current input the best are the ones that get to further strengthen their ability to detect these inputs. This is the essential insight that Hebb had with why the Hebbian learning function should strengthen an "engram".
    
* **homeostasis** to balance the positive feedback loop -- if left unchecked, the rich-get-richer dynamic ends up with a few units dominating everything, and as a result, all the inputs get categorized into one useless, overly-broad category ("everything"). The homeostatic mechanism in BCM helps fight against this by raising the floating threshold for highly active neurons, causing their weights to decrease for all but their most preferred input patterns, and thus restoring a balance. Similarly, under-active neurons experience net weight increases that get them participating and competing more effectively, and hence they come to represent distinct features.

The net result is the development of a set of neural detectors that relatively evenly cover the space of different inputs patterns, with systematic categories that encompass the statistical regularities. For example, cats like milk, and dogs like bones, and we can learn this just by observing the reliable co-occurrence of cats with milk and dogs with bones. This kind of reliable co-occurrence is what we mean by "statistical regularity". See Chapter Appendix on *Hebbian Learning* for a very simple illustration of why Hebbian-style learning mechanisms capture patterns of co-occurrence. It is really just a variant on the basic maxim that "things that fire together, wire together".

### The Learning Rate

There is an important factor missing from the above equations, which is the **learning rate** -- we typically use the greek epsilon $$\epsilon$$ to represent this parameter, which simply multiplies the rate with which the weights change:
$$ \Delta w = \epsilon f_{xcal}( xy, y_l) $$

Thus, a bigger epsilon means larger weight changes, and thus quicker learning, and vice-versa for a smaller value. A typical starting value for the learning rate is .04, and we often have it decrease over time (which is true of the brain as well -- younger brains are much more plastic than older ones) -- this typically results in the fastest overall learning and best final performance.

Many researchers (and drug companies) have the potentially dangerous belief that a faster learning rate is better, and various drugs have been developed that effectively increase the learning rate, causing rats to learn some kind of standard task faster than normal, for example. However, we will see in the Memory Chapter that actually a slow learning rate has some very important advantages. Specifically, a slower learning rate enables the system to incorporate *more statistics* into learning -- the learning rate determines the effective time window over which experiences are averaged together, and a slower learning rate gives a longer time window, which enables more information to be integrated. Thus, learning can be much smarter with a slower learning rate. But the tradeoff of course is that the results of this smarter learning take that much longer to impact actual behavior. Many have argued that humans are distinctive in our extremely protracted period of developmental learning, so we can learn a lot before we need to start earning a paycheck. This allows us to have a pretty slow learning rate, without too many negative consequences.

### Exploration of Self-Organizing Learning

The best way to see this dynamic is via the computational exploration. Open the `self_org` simulation from [CCN Sims](https://github.com/CompCogNeuro/sims) and follow the directions from there.

## Error-Driven Learning: Short Time Scale Floating Threshold

$\langle x y \rangle_m$ can produce error-driven learning. This medium time frame reflects the development of a pattern of neural activity that encodes an *expectation* about what will happen next. The most recent short term synaptic activity (which drives learning) represents the actual *outcome* of what did happen next. Because of the (nearly) linear nature of the dWt function, it effectively computes the difference between outcome and expectation. Qualitatively, if the outcome produces greater activation of a population of neurons than did expectation, corresponding weights go up, while neurons that decreased their activity states as a result of the outcome will have their weights go down. This is illustrated above in the case of low vs. high expectations.}}

$\langle x y \rangle_m$ versus the late phase of settling (the *plus phase* or short time frame activation average $\langle x y \rangle_s$). The late phase has integrated more of the overall constraints in the network and thus represents a "better" overall interpretation or representation of the current situation than the early phase, so it makes sense for the late phase to serve as the "training signal" relative to the earlier phase.}}

Although self-organizing learning is very useful, we'll see that it is significantly limited in the kinds of things that it can learn. It is great for extracting generalities, but not so great when it comes to learning specific, complicated patterns. To learn these more challenging types of problems, we need error-driven learning. For a more top-down (computationally motivated) discussion of how to achieve error-driven learning, and relationship to the more biologically motivated mechanisms we consider here, see the Chapter Appendix on  *Backpropagation* (which some may prefer to read first). Intuitively, error-driven learning is much more powerful because it drives learning based on *differences*, not *raw signals*. Differences (errors) tell you much more precisely what you need to do to fix a problem. Raw signals (overall patterns of neural activity) are not nearly as informative -- it is easy to become overwhelmed by the forest and lose sight of the trees. We'll see more specific examples later, after first figuring out how we can get error-driven learning to work in the first place.

shows how the same floating threshold behavior from the BCM-like self-organizing aspect of XCAL learning can be adapted to perform error-driven learning, in the form of differences between an **outcome vs. an expectation**. Specifically, we speed up the time scale for computing the floating threshold (and also have it reflect synaptic activity, not just receiver activity):
$$ \Theta_p = \langle xy \rangle_m $$
$$
\begin{aligned}
\Delta w & = f_{xcal}( \langle xy \rangle_s, \langle xy \rangle_m) \\
 & = f_{xcal}( x_s y_s, x_m y_m)
\end{aligned}
$$

where $\langle xy \rangle_m$ is this new medium-time scale average synaptic activity, which we think of as reflecting an emerging expectation about the current situation, which develops over approximately 75 msec of neural activity. The most recent, short-term (last 25 msec) neural activity ($\langle xy \rangle_s$) reflects the actual outcome, and it is the same calcium-based signal that drives learning in the Hebbian case.

In the simulator, the period of time during which this expectation is represented by the network, before it gets to see the outcome, is referred to as the **minus phase** (based on the *Boltzmann machine* terminology; [@AckleyHintonSejnowski85]. The subsequent period in which the outcome is observed (and the activations evolve to reflect the influence of that outcome) is referred to as the **plus phase**. It is the difference between this expectation and outcome that represents the error signal in error-driven learning (hence the terms minus and plus -- the minus phase activations are subtracted from those in the plus phase to drive weight changes).

Although this expectation-outcome comparison is the fundamental requirement for error-driven learning, a weight change based on this difference by itself begs the question of how the neurons would ever 'know' which phase they are in. We have explored many possible answers to this question, and the most recent involves an internally-generated alpha-frequency (10 Hz, 100 msec periods) cycle of expectation followed by outcome, supported by neocortical circuitry in the deep layers and the thalamus [@OReillyWyatteRohrlich14; @KachergisWyatteOReillyEtAl14]. A later revision of this textbook will describe this in more detail. For now, the main implications of this framework are to organize the timing of processing and learning as follows:

* A **Trial** lasts 100 msec (10 Hz, alpha frequency), and comprises one sequence of expectation -- outcome learning, organized into 4 quarters.
    + Biologically, the deep neocortical layers (layers 5, 6) and the thalamus have a natural oscillatory rhythm at the alpha frequency [@BuffaloFriesLandmanEtAl11; @LorinczKekesiJuhaszEtAl09; @FranceschettiGuatteoPanzicaEtAl95; @LuczakBarthoHarris13]. Specific dynamics in these layers organize the cycle of expectation vs. outcome within the alpha cycle.
        
* A **Quarter** lasts 25 msec (40 Hz, gamma frequency) -- the first 3 quarters (75 msec) form the expectation / minus phase, and the final quarter are the outcome / plus phase.

    + Biologically, the superficial neocortical layers (layers 2, 3) have a gamma frequency oscillation [@BuffaloFriesLandmanEtAl11], supporting the quarter-level organization.

*  A **Cycle** represents 1 msec of processing, where each neuron updates its membrane potential according to the equations covered in the Neuron Chapter.

The XCAL learning mechanism coordinates with this timing by comparing the most recent synaptic activity (predominantly driven by plus phase / outcome states) to that integrated over the medium-time scale, which effectively includes both minus and plus phases. Because the XCAL learning function is (mostly) linear, the association of the floating threshold with this synaptic activity over the medium time frame (including expectation states), to which the short-term outcome is compared, directly computes their difference:

$$ \Delta w \approx x_s y_s - x_m y_m $$

Intuitively, we can understand how this error-driven learning rule works by thinking about different specific cases. The easiest case is when the expectation is equivalent to the outcome (i.e., a correct expectation) -- the two terms above will be the same, and thus their subtraction is zero, and the weights remain the same. So once you obtain perfection, you stop learning. What if your expectation was higher than your outcome? The difference will be a negative number, and the weights will thus decrease, so that you will lower your expectations next time around. Intuitively, this makes perfect sense -- if you have an expectation that all movies by M. Night Shyamalan are going to be as cool as *The Sixth Sense*, you might end up having to reduce your weights to better align with actual outcomes. Conversely, if the expectation is lower than the outcome, the weight change will be positive, and thus increase the expectation. You might have thought this class was going to be deadly boring, but maybe you were amused by the above mention of M. Night Shyamalan, and now you'll have to increase your weights just a bit. It should hopefully be intuitively clear that this form of learning will work to minimize the differences between expectations and outcomes over time. Note that while the example given here was cast in terms of deviations from expectations having value (ie things turned out better or worse than expected, as we cover in more detail in the Motor control and Reinforcement Learning Chapter, the same principle applies when outcomes deviate from other sorts of expectations.

Because of its explicitly temporal nature, there are a few other interesting ways of thinking about what this learning rule does, in addition to the explicit timing defined above. To reiterate, the rule says that the outcome comes *immediately after* a preceding expectation -- this is a direct consequence of making it learn toward the short-term (most immediate) average synaptic activity, compared to a slightly longer medium-term average that includes the time just before the immediate present.

We can think of this learning in terms of the attractor dynamics discussed in the Networks Chapter. Specifically, the name **Contrastive Attractor Learning (CAL)** reflects the idea that the network is settling into an attractor state, and it is the contrast between the final attractor state that the network settles into (i.e., the "outcome" in this case), versus the network's activation trajectory as it approaches the attractor, that drives learning (). The short-time scale average reflects the final attractor state (the 'target'), and the medium time-scale average reflects the entire trajectory during settling. When the pattern of activity associated with the expectation is far from the actual outcome, the difference between these two attractor states will be large, and learning will drive weight changes so that in future encounters, the expectation will more closely reflect the outcome (assuming the environment is reliable). The **X** part of XCAL simply reflects the fact that the same objective is achieved without having to explicitly compare two attractors at discrete points in time, but instead by using a time-averaged activity state eXtended across the entire settling trajectory as the baseline comparison, which is more biologically realistic because such variables are readily accessible by local neuronal activity.

Mathematically, this CAL learning rule represents a simpler version of the oscillating learning function developed by Norman and colleagues -- see [@NormanNewmanDetreEtAl06; @RitvoTurk-BrowneNorman19].

There are also more general reasons for **later information** (short time scale average) to train **earlier information** (medium time scale average). Typically, the longer one waits, the better quality the information is -- at the start of a sentence, you might have some idea about what is coming next, but as it unfolds, the meaning becomes clearer and clearer. This later information can serve to train up the earlier expectations, so that you can more efficiently understand things next time around. Overall, these alternative ways of thinking about XCAL learning represent more self-organizing forms of learning without requiring an explicit outcome training signal, while using the more rapid contrast (short vs. medium time) for the error-driven learning mechanism.

Before continuing, you might be wondering about the biological basis of this error-driven form of the floating threshold. Unlike the BCM-style floating threshold, which has solid empirical data consistent with it, the idea that the threshold changes on this quicker time scale to reflect the medium time-scale average synaptic activity has not yet been tested empirically. Thus, it stands as an important prediction of this computational model. Because it is so easily computed, and results in such a powerful form of learning, it seems plausible that the brain would take advantage of just such a mechanism, but we'll have to see how it stands up to empirical testing. One initial suggestion of such a dynamic comes from this paper: [@LimMcKeeWoloszynEtAl15], which showed a BCM-like learning dynamic with rapid changes in the threshold depending on recent activity. Also, there *is* substantial evidence that transient changes in neuromodulation that occur during salient, unexpected events, are important for modifying synaptic plasticity -- and may functionally contribute to this type of error-driven learning mechanism. Also, we discuss a little bit later another larger concern about the nature and origin of the expectation vs. outcome distinction, which is central to this form of error-driven learning.

### Advantages of Error-Driven Learning

As noted above, error-driven learning is much more computationally powerful than self-organizing learning. For example, all computational models that perform well at the difficult challenge of learning to recognize objects based on their visual appearance (see the Perception Chapter) utilize a form of error-driven learning. Many also use self-organizing learning, but this tends to play more of a supporting role, whereas the models would be entirely non-functional without error-driven learning. Error-driven learning ensures that the model makes the kinds of categorical discriminations that are relevant, while avoiding those that are irrelevant. For example, whether a side view of a car is facing left or right is not relevant for determining that this is a car. But the presence of wheels is very important for discriminating a car from a fish. A purely self-organizing model has no way of knowing that these differences, which may be quite statistically reliable and strong signals in the input, differ in their utility for the categories that people care about.

Mathematically, the history of error-driven learning functions provides a fascinating window into the sociology of science, and how seemingly simple ideas can take a while to develop. In the Chapter Appendix on  Backpropagation, we trace this history through the derivation of error-driven learning rules, from the **delta rule** (developed by [@WidrowHoff60] to the very widely used **backpropagation** learning rule [@RumelhartHintonWilliams86]. At the start of that subsection, we show how the XCAL form of error-driven learning (specifically the CAL version of it) can be derived directly from backpropagation, thus providing a mathematically satisfying account as to why it is capable of solving so many difficult problems.

The key idea behind the backpropagation learning function is that error signals arising in an output layer can *propagate backward* down to earlier hidden layers to drive learning in these earlier layers so that it will solve the overall problem facing the network (i.e., it will ensure that the network can produce the correct expectations or answers on the output layer). This is essential for enabling the system as a whole to solve difficult problems -- as we discussed in the Networks Chapter, a lot of intelligence arises from multiple layers of cascading steps of categorization -- to get all of these intervening steps to focus on the relevant categories, error signals need to propagate across these layers and shape learning in all of them.

Biologically, the bidirectional connectivity in our models enables these error signals to propagate in this manner (). Thus, changes in any given location in the network radiate backward (and every which way the connections go) to affect activation states in all other layers, via bidirectional connectivity, and this then influences the learning in these other layers. In other words, XCAL uses bidirectional activation dynamics to communicate error signals throughout the network, whereas backpropagation uses a biologically implausible procedure that propagates error signals backward across synaptic connections, in the opposite direction of the way that activation typically flows. Furthermore, the XCAL network experiences a sequence of activation states, going from an expectation to experiencing a subsequent outcome, and learns on the difference between these two states. In contrast, backpropagation computes a single error *delta* value that is effectively the difference between the outcome and the expectation, and then sends this single value backwards across the connections. See the *Backpropagation* Appendix for how these two different things can be mathematically equivalent. Also, it is a good idea to look at the discussion of the **credit assignment** process in this subsection, to obtain a fuller understanding of how error-driven learning works.

### Exploration of Error-Driven Learning

The `pat_assoc` simulation from [CCN Sims](https://github.com/CompCogNeuro/sims) provides a nice demonstration of the limitations of self-organizing Hebbian-style learning, and how error-driven learning overcomes these limitations, in the context of a simple two-layer pattern associator that learns basic input/output mappings. Follow the directions in that simulation link to run the exploration.

You should have seen that one of the input/output mapping tasks was *impossible* for even error-driven learning to solve, in the two-layer network. The next exploration, `err_driven_hidden` shows that the addition of a hidden layer, combined with the powerful error-driven learning mechanism, enables even this "impossible" problem to now be solved. This demonstrates the computational power of the Backpropagation algorithm.

### Combined Self-Organizing and Error-Driven Learning

Although scientists have a tendency to want to choose sides strongly and declare that either self-organizing learning or error-driven learning is the best way to go, there are actually many advantages to combining both forms of learning together. Each form of learning has complementary strengths and weaknesses:

* Self-organizing is more *robust*, because it only depends on local statistics of firing, whereas error-driven learning implicitly depends on error signals coming from potentially distant areas. Self-organizing can achieve something useful even when the error signals are remote or not yet very coherent.
    
* But self-organizing learning is also very *myopic* -- it does not coordinate with learning in other layers, and thus tends to be "greedy". In contrast, error-driven learning achieves this coordination, and can learn to solve problems that require collective action of multiple units across multiple layers.

One analogy that may prove useful is that error-driven learning is like left-wing politics -- it requires all the different layers and units to be working together to achieve common goals, whereas self-organizing learning is like right-wing politics, emphasizing local, greedy actions that somehow also benefit society as a whole, without explicitly coordinating with others. The tradeoffs of these political approaches are similar to those of the respective forms of learning. Socialist approaches can leave individual people feeling not very motivated, as they are just a little cog in a huge faceless machine. Similarly, neurons that depend strictly on error-driven learning can end up not learning very much, as they only need to make a very small and somewhat "anonymous" contribution to solving the overall problem. Once the error signals have been eliminated (i.e., expectations match outcomes), learning stops. We will see that networks that rely on pure error-driven learning often have very random-looking weights, reflecting this minimum of effort expended toward solving the overall problem. On the other side, more strongly right-wing capitalist approaches can end up with excessive positive feedback loops (rich get ever richer), and are typically not good at dealing with longer-term, larger-scale problems that require coordination and planning. Similarly, purely self-organizing models tend to end up with more uneven distributions of "representational wealth" and almost never end up solving challenging problems, preferring instead to just greedily encode whatever interesting statistics come their way. Interestingly, our models suggest that a balance of both approaches -- a *centrist* approach -- seems to work best! Perhaps this lesson can be generalized back to the political arena.

Colorful analogies aside, the actual mechanics of combining both forms of learning within the XCAL framework amounts to merging the two different definitions of the floating threshold value. Biologically, we think that there is a combined weighted average of the two thresholds, using a "lambda" parameter $\lambda$ to weight the long-term receiver average (self-organizing) relative to the medium-term synaptic co-product:
$$ \theta_p = \lambda y_l + (1-\lambda) x_m y_m $$

However, computationally, it is clearer and simpler to just combine separate XCAL functions, each with their own weighting function -- due to the linearity of the function, this is mathematically equivalent:

$$ \Delta w = \lambda_l f_{xcal} ( x_s y_s, y_l) + \lambda_m f_{xcal} (x_s y_s, x_m y_m) $$

It is reasonable that these lambda parameters may differ according to brain area (i.e., some brain systems learn more about statistical regularities, whereas others are more focused on minimizing error), and even that it may be dynamically regulated (i.e. transient changes in neuromodulators like dopamine and acetylcholine can influence the degree to which error signals are emphasized).

There are small but reliable computational advantages to automating this balancing of self-organizing vs. error-driven learning (i.e., a dynamically-computed $\lambda_l$ value, while keeping $\lambda_m = 1$), based on two factors: the magnitude of the $y_l$ receiving-unit running average activation, and the average magnitude of the error signals present in a layer (see *Leabra Details* Chapter Appendix).

### Weight Bounding and Contrast Enhancement

The one last issue we need to address computationally is the problem of synaptic weights growing without bound. In LTP experiments, it is clear that there is a maximum synaptic weight value -- you cannot continue to get LTP on the same synapse by driving it again and again. The weight value **saturates**. There is a natural bound on the lower end, for LTD, of zero. Mathematically, the simplest way to achieve this kind of weight bounding is through an **exponential approach** function, where weight changes become exponentially smaller as the bounds are approached. This function is most directly expressed in a programming language format, as it involves a conditional:

```
if   dWt > 0 then Wt = Wt + (1 - Wt) * dWt
else              Wt = Wt + Wt * dWt
```

In words: if weights are supposed to increase (dwt is positive), then multiply the rate of increase by 1-wt, where 1 is the upper bound, and otherwise, multiply by the weight value itself. As the weight approaches 1, the weight increases get smaller and smaller, and similarly as the weight value approaches 0.

6, offset (theta) {{=}} 1.25.}}

The exponential approach function works well at keeping weights bounded in a graded way (much better than simply clipping weight values at the bounds, which loses all the signal for saturated weights), but it also creates a strong tendency for weights to hang out in the middle of the range, around .5. This creates problems because then neurons don't have sufficiently distinct responses to different input patterns, and then the inhibitory competition breaks down (many neurons become weakly activated), which then interferes with the positive feedback loop that is essential for learning, etc. To counteract these problems, while maintaining the exponential bounding, we introduce a **contrast enhancement** function on the weights:

$$ \hat{w} = \frac{1}{1 + \left(\frac{w}{\theta (1-w)}\right)^{-\gamma}} $$

As you can see in , this function creates greater contrast for weight values around this .5 central value -- they get pushed up or down to the extremes. This contrast-enhanced weight value is then used for communication among the neurons, and is what shows up as the wt value in the simulator.

Biologically, we think of the plain weight value w, which is involved in the learning functions, as an *internal* variable that accurately tracks the statistics of the learning functions, while the contrast-enhanced weight value is the actual synaptic efficacy value that you measure and observe as the strength of interaction among neurons. Thus, the plain w value may correspond to the phosphorylation state of CAMKII or some other appropriate internal value that mediates synaptic plasticity.

Finally, see the *Implementational Details* Appendix for a few implementational details about the way that the time averages are computed, which don't affect anything conceptually, but if you really want to know *exactly* what is going on..

## When, Exactly, is there an Outcome that should Drive Learning?

This is the biggest remaining question for error-driven learning. You may not have even noticed this issue, but once you start to think about implementing the XCAL equations on a computer, it quickly becomes a major problem. We have talked about how the error-driven learning reflects the difference between an outcome and an expectation, but it really matters that the short-term average activation representing the outcome state reflects some kind of actual outcome that is worth learning about. illustrates four primary categories of situations in which an outcome state can arise, which can play out in myriad ways in different real-world situations.

In our most recent framework described briefly above, the expectation-outcome timing is specified in terms of the 100 msec alpha trial. And within this trial, the combined circuitry between the deep neocortical layers and the thalamus end up producing an outcome state that drives *predictive auto-encoder* learning, which is basically the last case (d) in , with an extra twist that during every 100 msec alpha trial, the network attempts to predict what will happen in the next 100 msec -- the predictive aspect of the auto-encoder idea. Specifically, the deep layers attempt to predict what the bottom-up driven activity pattern over the thalamus will look like in the final plus-phase quarter of the alpha trial, based on activations present during the prior alpha trial. Because of the extensive bidirectional connectivity between brain areas, the cross-modal expectation / output sequence shown in panel (b) of is also supported by this mechanism. A later revision of this text will cover these ideas in more detail. Preliminary versions are available: [@OReillyWyatteRohrlich17; @KachergisWyatteOReillyEtAl14].

Another hypothesis for something that "marks" the presence of an important outcome is a phasic burst of a neuromodulator like **dopamine**. It is well established that dopamine bursts occur when an unexpected outcome arises, at least in the context of expectations of reward or punishment (we'll discuss this in detail in the Motor Control and Reinforcement Learning Chapter. Furthermore, we know from a number of studies that dopamine plays a strong role in modulating synaptic plasticity. Under this hypothesis, the cortical network is always humming along doing standard BCM-like self-organizing learning at a relatively low learning rate (due to a small lambda parameter in the combined XCAL equation, which presumably corresponds to the rate of synaptic plasticity associated with the baseline tonic levels of dopamine), and then, when something unexpected occurs, a dopamine burst drives stronger error-driven learning, with the immediate short-term average "marked" by the dopamine burst as being associated with this important (salient) outcome. The XCAL learning will automatically contrast this immediate short-term average with the immediately available medium-term average, which presumably reflects an important contribution from the prior expectation state that was just violated by the outcome.

There are many other possible ideas for how the time for error-driven learning is marked, some of which involve local emergent dynamics in the network itself, and others that involve other neuromodulators, or networks with broad connectivity to broadcast an appropriate "learn now" signal. From everything we know about the brain, there are likely several such learning signals, each of which being useful in some particular subset of situations. This is an active area of ongoing research.

## The Leabra Framework

provides a summary of the **Leabra** framework, which is the name given to the combination of all the neural mechanisms that have been developed to this point in the text. Leabra stands for *Learning in an Error-driven and Associative, Biologically Realistic Algorithm* -- the name is intended to evoke the "Libra" balance scale, where in this case the balance is reflected in the combination of error-driven and self-organizing learning ("associative" is another name for Hebbian learning). It also represents a balance between low-level, biologically-detailed models, and more abstract computationally-motivated models. The biologically-based way of doing error-driven learning requires bidirectional connectivity, and the Leabra framework is relatively unique in its ability to learn complex computational tasks in the context of this pervasive bidirectional connectivity. Also, the FFFB inhibitory function producing k-Winners-Take-All dynamics is unique to the Leabra framework, and is also very important for its overall behavior, especially in managing the dynamics that arise with the bidirectional connectivity.

The different elements of the Leabra framework are therefore synergistic with each other, and as we have discussed, highly compatible with the known biological features of the neocortex. Thus, the Leabra framework provides a solid foundation for the cognitive neuroscience models that we explore next in the second part of the text.

For those already familiar with the Leabra framework, see the Leabra Details Appendix for a brief review of how the XCAL version of the algorithm described here differs from the earlier form of Leabra described in the original *Computational Explorations in Cognitive Neuroscience* textbook [@OReillyMunakata00].

### Exploration of Leabra

Open the `family_trees` simulation in [CCN Sims](https://github.com/CompCogNeuro/sims) to explore Leabra learning in a deep multi-layered network running a more complex task with some real-world relevance. This simulation is very interesting for showing how networks can create their own similarity structure based on functional relationships, refuting the common misconception that networks are driven purely by input similarity structure.

## Appendix

Here are all the sub-topics within the Learning chapter, collected in one place for easy browsing. These may or may not be optional for a given course, depending on the instructor's specifications of what to read:

* **Detailed Biology of Learning:** more in-depth treatment of postsynaptic signaling cascades that mediate LTP and LTD, described in context of the [@UrakuboHondaFroemkeEtAl08] model of synaptic plasticity.
    
* **Hebbian Learning:** extensive treatment of computational properties of Hebbian learning -- starts with a simple manual simulation of Hebbian learning showing exactly how and why it captures patterns of co-occurrence.
    
* **STDP:** more details on spike timing dependent plasticity.

* **Backpropagation:** history and mathematical derivation of error-driven learning functions -- strongly recommended to obtain greater insight into the computational nature of error-driven learning (starts with some important conceptual points before getting into the math).
    
* **Implementational Details:** misc implementational details about how time averaged activations are computed, etc.
    
* **Leabra Details:** describes how the current version of Leabra based on XCAL differs from the original one from the *Computational Explorations in Cognitive Neuroscience* textbook [@OReillyMunakata00].
    
* Full set of Leabra equations on *emergent* [leabra](https://github.com/emer/leabra) site.

## Detailed Biology of Learning
    
## Hebbian Learning
    
## STDP

## Backpropagation
    
## Implementational Details
    
## Leabra Details
