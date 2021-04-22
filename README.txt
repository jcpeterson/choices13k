***********************************************************************
*                         choices13k Dataset                          *
***********************************************************************

Summary
-------
The choices13k dataset consists of human gamble selection frequencies for
13,006 risky choice problems generated using the procedure described in Erev et
al. (2017), Appendix D. 

Description
-----------
Each choice problem required a participant to select the most appealing gamble
from a set of two possible gambles, A or B. Both gambles were represented to
participants as a list of rewards and their associated outcome probabilities.
Gamble A was constrained to have only two outcomes. Gamble B yielded a fixed
reward with probability 1 - pL and the outcome of a lottery (i.e., the outcome
of another explicitly described gamble) otherwise. Gamble B's lottery varied by
problem. For a detailed description of the gamble presentation and
parameterization, see Erev et al.  (2017), Appendices A & C.

The presentation format for the gambling problems closely followed that of the
2015 and 2018 Choice Prediction Competitions [1], with the exception that the
block parameter in CPC15 and CPC18 was reduced from five to two, and feedback
and no-feedback blocks were not required to be presented sequentially.  We
found that this relaxation did not significantly affect overall predictive
accuracy in our models, and allowed us to more than halve the number of trials
necessary for each problem.

From [1]:

    In particular, each problem in the space is a choice between Option A,
    which provides Ha with probability pHA or LA otherwise (with probability 1
    >= pHA), and Option B, which provides a lottery (that has an expected value
    of HB) with probability pHB and provides LB otherwise (with probability 1 >=
    pHB).  The distribution of the lottery around its expected value (Hb) is
    determined by the parameters LotNum (which defines the number of possible
    outcomes in the lottery) and LotShape (which defines whether the
    distribution is symmetric around its mean, right-skewed, left-skewed, or
    undefined if LotNum = 1), as explained in Appendix A. The Corr parameter
    determines whether there is a correlation (positive, negative, or zero)
    between the payoffs of the two options. The tenth parameter, ambiguity
    (Amb), captures the precision of the initial information the decision maker
    receives concerning the probabilities of the possible outcomes in Option B.
    We focus on the two extreme cases: Amb ⫽ 1 implies no initial information
    concerning these probabilities (they are described with undisclosed fixed
    parameters), and Amb ⫽ 0 implies complete information and no ambiguity (as
    in Allais, 1953; Kahneman & Tversky, 1979).  The eleventh dimension in the
    space is the amount of feedback the decision maker receives after making a
    decision.  As Table 1 shows, some phenomena emerge in decisions without
    feedback (i.e., decisions from description), and other phenomena emerge
    when the decision maker can rely on feedback (i.e., decisions from
    experience).  We studied this dimension within problem. That is, decision
    makers faced each problem first without feedback, and then with full
    feedback (i.e., realization of the obtained and forgone outcomes
    following each choice).


References
----------
[1] Erev, I., Ert, E., Plonsky, O., Cohen, D., and Cohen, O. From anomalies to
forecasts: Toward a descriptive model of decisions under risk, under
ambiguity, and from experience. Psychological Review, 124(4):369-409, 2017.

[2] Plonsky, O., Apel, R., Erev, I., Ert, E., and Tennenholtz, M. When and how
can social scientists add value to data scientists? A choice prediction
competition for human decision making. Unpublished Manuscript, 2018.


Column Descriptions
-------------------
problem : INT
    A unique internal problem ID
n : INT
    The number of subjects run on the current problem
feedback : BOOLEAN
    Whether the participants were given feedback about the reward they received
    and missed out on after making their selection.
bRate : FLOAT
    The frequency with which subjects selected Gamble B on the current problem.
target_b* : FLOAT
    These columns contain the predicted bRate probabilities from the BEAST
    model for blocks 1-5 of the current problem.
Ha : INT
    The first outcome of gamble A
pHa : FLOAT
    The probability of Ha in gamble A
La : INT
    The second outcome of gamble A (occurs with probability 1-pHa).
Hb : INT
    The expected value of the lottery in gamble B
pHb : FLOAT
    The probability of outcome Lb in gamble B
Lb : INT
    The non-lottery outcome for gamble B (occurs with probability pHb)
LotShapeB : CATEGORICAL {0, 1, 2, 3}
    The shape of the lottery distribution for gamble B. A value of X indicates
    the distribution is symmetric around its mean, X indicates right-skewed, X
    indicates left-skewed, and 3 indicates undefined (i.e., if LotNumB = 1).
LotNumB : INT
    The number of possible outcomes in the gamble B lottery
Amb : BOOLEAN {0, 1}
    Whether the decision maker was able to see the probabilities of the
    outcomes in Gamble B. 1 indicates the participant received no information
    concerning the probabilities, and 0 implies complete information and no
    ambiguity.
Corr : CATEGORICAL {-1, 0, 1}
    Whether there is a correlation (negative, zero, or positive) between the
    payoffs of the two gambles.
Block : CATEGORICAL {1, 2, 3, 4, 5}
