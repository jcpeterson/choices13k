# choices13k Dataset

The choices13k dataset consists of aggregate human selection frequencies on
13,006 risky choice problems collected via Mechanical Turk. 

## Description

Each choice problem required a participant to select the most appealing option
from a set of two decision scenarios ("gambles"). Both gambles were represented
to participants as a list of rewards and their associated probabilities. In
each problem, one gamble ("Gamble A") was constrained to have only two
outcomes. The other, "Gamble B," yielded a fixed reward with probability 1 - pL
and the outcome of a lottery (i.e., the outcome of another explicitly described
gamble) otherwise. Gamble B's lottery varied by problem. For a detailed
description of the gamble presentation and parameterization, see
[[1](http://oriplonsky.com/wp-content/uploads/2017/09/Erev-et-al-2017.pdf)],
Appendices A & C.

The presentation format for the gambling problems closely followed that of the
2015 and 2018 Choice Prediction Competitions
[[1](http://oriplonsky.com/wp-content/uploads/2017/09/Erev-et-al-2017.pdf),
[2](https://cpc18.files.wordpress.com/2018/01/cpc18-white-paper.pdf)] with the
exception that the block parameter in CPC15 and CPC18 was reduced from five to
two, and feedback and no-feedback blocks were not required to be presented
sequentially.  We found that this relaxation did not significantly affect
overall predictive accuracy in our models, and allowed us to more than halve
the number of trials necessary for each problem.

From [[1](http://oriplonsky.com/wp-content/uploads/2017/09/Erev-et-al-2017.pdf)]:

> In particular, each problem in the space is a choice between Option A, which
> provides Ha with probability pHA or LA otherwise (with probability 1 >= pHA),
> and Option B, which provides a lottery (that has an expected value of HB)
> with probability pHB and provides LB otherwise (with probability 1 >= pHB).
> The distribution of the lottery around its expected value (Hb) is determined
> by the parameters LotNum (which defines the number of possible outcomes in
> the lottery) and LotShape (which defines whether the distribution is
> symmetric around its mean, right-skewed, left-skewed, or undefined if LotNum
> = 1), as explained in Appendix A. The Corr parameter determines whether there
> is a correlation (positive, negative, or zero) between the payoffs of the two
> options. The tenth parameter, ambiguity (Amb), captures the precision of the
> initial information the decision maker receives concerning the probabilities
> of the possible outcomes in Option B.  We focus on the two extreme cases: Amb
> = 1 implies no initial information concerning these probabilities (they are
> described with undisclosed fixed parameters), and Amb = 0 implies complete
> information and no ambiguity (as in Allais, 1953; Kahneman & Tversky, 1979).
> The eleventh dimension in the space is the amount of feedback the decision
> maker receives after making a decision.  As Table 1 shows, some phenomena
> emerge in decisions without feedback (i.e., decisions from description), and
> other phenomena emerge when the decision maker can rely on feedback (i.e.,
> decisions from experience).  We studied this dimension within problem. That
> is, decision makers faced each problem first without feedback, and then with
> full feedback (i.e., realization of the obtained and forgone outcomes
> following each choice).

## Data Format

The columns of `c13k_selections.csv` are:

|   Column  |    Data Type    | Description                                                                                                                                                                                                                      |
|:---------:|:---------------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  problem  |     Integer     | A unique internal problem ID                                                                                                                                                                                                     |
|     n     |     Integer     | The number of subjects run on the current problem                                                                                                                                                                                |
|  feedback |     Boolean     | Whether the participants were given feedback about the reward they received and missed out on after making their selection.                                                                                                      |
|   bRate   |      Float      | The frequency with which subjects selected Gamble B on the current problem.                                                                                                                                                      |
|  BEAST_*  |      Float      | The predicted bRate probabilities from the BEAST model for blocks 1-5 of the current problem.                                                                                                                                    |
|     Ha    |     Integer     | The first outcome of gamble A                                                                                                                                                                                                    |
|    pHa    |      Float      | The probability of Ha in gamble A                                                                                                                                                                                                |
|     La    |     Integer     | The second outcome of gamble A (occurs with probability 1-pHa).                                                                                                                                                                  |
|     Hb    |     Integer     | The expected value of the lottery in gamble B                                                                                                                                                                                    |
|    pHb    |      Float      | The probability of outcome Lb in gamble B                                                                                                                                                                                        |
|     Lb    |      Float      | The non-lottery outcome for gamble B (occurs with probability pHb)                                                                                                                                                               |
| LotShapeB |   {0, 1, 2, 3}  | The shape of the lottery distribution for gamble B. A value of 1 indicates the distribution is symmetric around its mean, 2 indicates right-skewed, 3 indicates left-skewed, and 0 indicates undefined (i.e., if LotNumB = 1).    |
|  LotNumB  |     Integer     | The number of possible outcomes in the gamble B lottery                                                                                                                                                                          |
|    Amb    |     Boolean     | Whether the decision maker was able to see the probabilities of the outcomes in Gamble B. 1 indicates the participant received no information concerning the probabilities, and 0 implies complete information and no ambiguity. |
|    Corr   |    {-1, 0, 1}   | Whether there is a correlation (negative, zero, or positive) between the payoffs of the two gambles.                                                                                                                             |
|   Block   | {1, 2, 3, 4, 5} | The "block ID" for the current problem. For problems with no feedback, Block is always 1. Otherwise, Block ID was sampled uniformly at random from {2, 3, 4, 5}.                                                                 |

The specific payoff amounts and probabilities displayed on each problem is contained in `c13k_problems.json`. Keys correspond to the row index in `c13k_selections.csv`. The first entry looks like:

```
{
    "0" : {
        "A": [[0.95, 26.0], [0.05, -1.0]],
        "B": [[0.95, 21.0], [0.05, 23.0]]
    }
}
```

which indicates that gambles associated with index 0 in `c13k_selections.csv` were:

```
        Gamble A                      Gamble B
   Payout  Probability          Payout  Probability
     26.0         0.95            21.0         0.95
     -1.0         0.05            23.0         0.05
```

## Citation

If you use choices13k in your research, please cite:

> Peterson, J. C., Bourgin, D. D., Agrawal, M., Reichman, D., and Griffiths, T. J. [Using large-scale experiments and machine learning to discover theories of human decision-making](https://science.sciencemag.org/content/372/6547/1209/). _Science(372): 1209-1214_, 2021.

```
@article {Peterson2021a,
	author = {Peterson, Joshua C. and Bourgin, David D. and Agrawal, Mayank and Reichman, Daniel and Griffiths, Thomas L.},
	title = {Using large-scale experiments and machine learning to discover theories of human decision-making},
	volume = {372},
	number = {6547},
	pages = {1209--1214},
	year = {2021},
	doi = {10.1126/science.abe2629},
	issn = {0036-8075},
	journal = {Science}
}
```

## References

[1] Erev, I., Ert, E., Plonsky, O., Cohen, D., and Cohen, O. [From anomalies to
forecasts: Toward a descriptive model of decisions under risk, under
ambiguity, and from experience](http://oriplonsky.com/wp-content/uploads/2017/09/Erev-et-al-2017.pdf). _Psychological Review, 124(4):369-409_, 2017.

[2] Plonsky, O., Apel, R., Erev, I., Ert, E., and Tennenholtz, M. [When and how
can social scientists add value to data scientists? A choice prediction
competition for human decision making](https://cpc18.files.wordpress.com/2018/01/cpc18-white-paper.pdf). Unpublished Manuscript, 2018.
