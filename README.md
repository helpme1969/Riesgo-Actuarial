# Riesgo-Actuarial
Se necesita script de este problema, leguaje Rstudio
' So the problem goes as follows:

"Suppose that the different policyholders of a casualty insurance company generate claims according to independent Poisson processes with a common rate λ, and that each claim amount has distribution F. Suppose also that new customers sign up according to a Poisson process with rate ν, and that each existing policyholder remains with the company for an exponentially distributed time with rate μ. Finally, suppose that each policyholder pays the insurance firm at a fixed rate c per unit time. Starting with n0 customers and initial capital a0≥0, we are interested in using simulation to estimate the probability that the firm’s capital is always nonnegative at all times up to time T ."

Well to simulate the preceding, we define the variables and events as follows:

Time Variable: t

System State Variable: (n,a), where n is the number of policyholders and a is the firm’s current capital.

Events: There are three types of events: a new policyholder, a lost policyholder, and a claim. The event list consists of a single value, equal to the time at which the next event occurs.

EL: tE
We are able to have the event list consist solely of the time of the next event because of results about exponential random variables. Specifically, if (n,a) is the system state at time t then, because the minimum of independent exponential random variables is also exponential, the time at which the next event occurs will equal t+X, where X is an exponential random variable with rate ν+nμ+nλ. Moreover, no matter when this next event occurs, it will result from

A new policyholder, with probability vν+nμ+nλ

A lost policyholder, with probability nμν+nμ+nλ

A claim, with probability nλν+nμ+nλ

After determining when the next event occurs, we generate a random number to determine which of the three possibilities caused the event, and then use this information to determine the new value of the system state variable.

In the following, for given state variable (n,a), X will be an exponential random variable with rate ν+nμ+nλ; J will be a random variable equal to 1 with probability vν+nμ+nλ, to 2 with probability nμν+nμ+nλ, or to 3 with probability nλν+nμ+nλ; Y will be a random variable having the claim distribution F.

Output Variable: I, where

I=1, if the firm’s capital is nonnegative throughout [0,t]

I=0, otherwise

Thinking of the algorithm it will look something like this:

Initialize

First initialize t = 0, a = a0, n = n0

then generate X and initialize t_E = X

To update the system we move along to the next event, first checking whether it takes us past time T .

Update Step

Case 1: t_e > T: Set I = 1 and end this run.

Case 2: t_e <= T: Reset a = a + nc(t_e − t) and t = t_e

Generate J: If J = 1: reset n = n + 1 then if J = 2: reset n = n − 1 then if J = 3: Generate Y. if Y > a, set I = 0 and end this run; otherwise reset a = a − Y

Generate X: reset t_e = t + X

The update step is then continually repeated until a run is completed.

So far I think this could work, but since I'm not closely familiar with actuarial science (and insurance companies in general) my questions are:

What values for a0 and n0 are the most suitable (realistic) for this model in actuarial science?
What distributions F for my Y RV are common and uncommon in this field?
What range and in wich units of time [0,t] are generally used to test models like this?
My goal is to simulate this model on python and compare if changing the distributions or rates around could determine whether this company go bankruptcy or stays with a nonnegative capital over a defined time.

PS: Please let me know what else should be taking in cosideration. Is this is just a trial and error problem (change values, variables, distribution, etc to look for a change in the end result)?.'
