# Forward Starting CDO Valuation



The forward starting CDO (FSCDO) valuation model serves the purpose of pricing a forward starting CDO (FSCDO) tranche. An FSCDO trade is defined as an agreement to enter into a CDO trade at some time in the future. Unlike a usual forward starting instrument in the interest rate world, the obligors in the collateral pool of the FSCDO may default before the forward starting date, which makes the pricing of such trades complicated. Furthermore, the FSCDO involves the forward start date and the trade maturity; hence a pertinent valuation model needs to be calibrated to the market implied correlation information with a full term structure.

In the past three years the structured credit derivative modeling has witnessed the emergence of the vanilla CDO trades pricing using market implied correlation information, and the standardization of the normal copula model within the base correlation framework.  The occurrence of the more complicated structure, such as the FSCDO trade discussed in this report, leads to alternative ways of explaining the correlation information implied by the market standard iTraxx and CDX indices.  In the initial modeling approach, a copula is normally assumed first, for example normal copula, t copula, etc. The random factor loading approach relaxes this assumption to some extent by assuming the factor as a function of the latent variable hence the joint marginal distribution is no longer normal. The latest development moves even further. As proposed by several authors, a non-parametric modeling framework is employed to match the market information. For example, in the perfect copula model, the assumption of any copula form is dropped, and the copula function itself is implied by the market information. In the Gaussian mixture model, three correlation values and their corresponding weights are found to match five market quotations of an iTraxx or CDX index.

The FSCDO model belongs to the non-parametric category.  In order to price the FSCDO with the market implied information of different terms, the model employs a WMC method. The following procedures are involves:

1) Generation of the Prior correlated Default Scenarios via Monte Carlo (MC) Simulation
	
By using the normal copula MC simulation model, the prior correlated default scenarios of the obligor in the collateral pool are simulated with a set of correlation parameters. Currently we choose five correlation values 0, 0.2, 0.4, 0.6, and 0.95, with the minimum number of simulation associated with each correlation being 100,000.  The correlated default times of obligors in each MC scenario are recorded. 
	
2) Computation of the Optimized Weight of Each MC Scenario

The path weight of each MC scenario is optimized such that both the unconditional default probabilities of the obligors and the spot market implied correlation information can be reproduced. The least set of constraints in the optimization includes two subsets. One is the spot unconditional default probabilities of each reference name with a full term structure between the forward starting date and maturity; the other subset is the spot tranche expected losses at the forward starting date and maturity, normalized by the corresponding expected losses of the collateral pool. The choices of the number of tranches and corresponding tranching position are determined on a trade by trade basis. 


3). Valuation of the FSCDO and Computation of Sensitivities

For each generated correlated default scenario, the pay off of the FSCDO is calculated. Then the value of the trade is computed by summing all the scenarios weighted with the corresponding path weights, calibrated in step 2.

In general the sensitivities are calculated by shocking the appropriate risk factors and revaluing. Except for the correlation sensitivity, the model is recalibrated by holding the input base correlations unchanged, which is consistent with existing sensitivity computation for all other structured credit products.

There are two advantages of the model. First, like other non-parametric modeling approaches, the model ensures an arbitrage free pricing model once successfully calibrated. Moreover, as shown in the next section, the model can be calibrated to the market information with at least two different maturities, which can be viewed as a progress of the non-parametric modeling. As far as we know, all the other non-parametric modeling can only be calibration to market data with one maturity.

However, in the model there are two model uncertainties due to the feature of the WMC simulation:

•	The Choice of Correlations to Generate Prior MC Scenarios

Each prior MC scenario has to be generated with a given correlation value.  Due to the existence of market implied correlation skew, in practical the MC scenario generated by any single correlation value won’t be able to calibrate to the market, hence a set of correlation values has to be selected. Two different choices of the correlation set would generate different sets of correlated default scenarios hence predict different value of the FSCDO, even if both can be calibrated the same spot tranche values quite satisfactorily. 

•	The Calibration Precision and Choice of Calibrating Tranches

Unlike other non-parametric approaches, the calibration target in the model is the weight of the MC simulation path hence the MC noise is fundamentally embedded. More importantly, the theory of the MC simulation requires that the calibrated MC scenarios should reprice the credit spreads (or, in other words, the unconditional default probabilities) of each obligor in the collateral pool. Hence in addition to the spot tranche values, the credit spreads of each obligor become the calibrating instruments. The number of the calibrating instruments becomes large if there are many obligors in the collateral pool, making the calibration difficult and time consuming. Normally a calibration error tolerance control has to be imposed to maintain both precision and efficiency.  



Another calibration uncertainty resides in the choice of calibrating tranches. First, like with all other non-parametric approaches a good fit of all market quotes of the indexes is unlikely without further assumptions outside of the current modeling framework. Second, a different choice of the capital structure and attachment and detachment points of the spot calibrating tranches will predict different FSCDO values. A procedure on how to choose the calibrating instruments has been proposed by GSO FO and attached in Appendix III.

Because the above uncertainties are fundamentally embedded in the model, an effective way of managing the model uncertainties is to set up an appropriate model risk reserve.  GMRMR London and GMRMR QA have agreed that, for each FSCDO trade, two most conservative scenarios are searched by 1) using various calibration instruments with the different levels of calibration tolerance, and 2) using various copulas and correlations to generate prior MC scenario. The model risk reserve is then determined by a combination of the above two. 
	
Due to the nature of the MC simulation and the non-parametric approach in the model, it is unclear whether a particular scenario is conservative or not. According to our understanding to the model, possible scenarios could include:

–	The calibrating tranches are set to be the standard index tranches such as 3%, 7%, 10%, etc. for CDX, a series of tranches with the same thickness as the target FSCDO tranche,  and a series of tranchlet starting from the equity tranche.
–	The calibration error tolerance is tightened to  or more stringent.
–	Many combinations of the correlation values are tested to find the most conservative prior correlation set.
–	The default correlation model can be the normal copula model, the Poisson model, or a mixture of the two.

When we begin to accumulate positions in FSCDOs, well defined conservative scenarios, against which a reasonable model risk reserve can be set up, will be finalized.

According to the market convention, any obligor defaulted before the forward starting date is replaced with an AAA rated obligor. The presence of the replacement option in the FSCDO trade makes the pricing complicated within the current pricing framework. From the viewpoint of modeling, the replacement mechanism is an embedded path dependent. It can not be properly valued without any modeling of a dynamical loss process. However, the option can be largely ignored by treating the replaced AAA rated obligor as a risk free asset. In this case, as shown in the pay off function in the next section, the practical implementation of this feature is modeled by ignoring any default event before the forward starting date. 

In the following of the report, the collateral pool of a CDO trade is defined as a set of M reference names, , in which each reference name is described by a hazard rate curve  , recovery rate  , a notional amount  ,  and a default time  .  The FSCDO trade is defined as a tranche with an attachment point A, a detachment point D, and a forward starting time S, fixed time horizon , and forward starting fee payments at interval .  

In order to price an FSCDO, we first write the spot accumulated loss function of the reference tranche at time t as follow:

(1)	 ,
where is the total accumulated loss function of the reference pool over time.  is the associated loss given default of the nth obligor. 

For an FSCDO trade starting from S to T, the loss function can be written as

(2)	 ,

Note that, by this definition, any default events before the forward start date is ignored and replaced with a risk free asset [6]. 

Once we know Eq.(2), the value of the FSCDO can be computed. The value of protection and premium legs read:

Value of Protection:

(3)		 .

Annuity of the fee leg:

 (4)		 ,

with the first term being the annuity of the fee payment stream and the second the fee accrual. Payment for the fee during the period   is made at ;   is the day count fraction; D(t) is the discount factor.

In the actual trade the fee payments start when the FSCDO is settled. Hence the b/s spread calculated using the Eqs. (3) and (4) are not the traded spread for the FSCDO. Because the trade cannot be hit by any default before the forward starting date, the notional amount of will remain unchanged hence value of the fee leg is actually a risk free annuity. This part of the fee leg value can be easily calculated and added to Eq.(4). Hence the actual b/e spread for the FSCDO can be calculated readily.

In the model, a WMC is employed. Instead of directly calculating the expected loss function of the FSCDO, as shown in Eq.(2), the correlated default events are simulated. In each MC scenario, the LGDs are allocated to the tranches by seniority of the tranche and then discounted. For the FSCDO, any default before the forward starting date is simply ignored. The present value of the FSCDO is calculated by summing all MC scenarios.

The MC simulation can be divided into two categories: those are uniformly distributed and non-uniformly distributed. A conventional MC simulation algorithm assigns each simulated MC scenario, or path, an identical probability weight.  On the other hand, we can allow a different probability to be assigned to each simulation path.

To see how the later method works, consider that a contingent claim has a value    for MC scenario  .  The value computed according to equally-weighted MC simulation will be given by

(5)		  

whereas for non-uniformly weighted Monte Carlo we have

(6)		  

where   is the probability weight for path  .    

A method of computing the probability weights   has to be specified.    Consider   benchmark instruments  .  Let the value of the   benchmark instrument in the   simulation path be denoted  .  Then, in order to match the benchmark instruments, as required,  we must have

(7)		  

for all  .

According the theory of the MC simulation, the right side of Eqs. (6) and (7)  are the expected values if the benchmark instruments are used to generate the MC paths. In the structure credit derivative modeling, these instruments are credit spreads or unconditional default probabilities of the obligors in the collateral pool (see, for example, Ref [7]).

Because a finite number of paths are used for any MC simulation, a MC noise always exists, that is, the right side of Eqs. (6) and (7) can not be automatically met due to the MC noise. The larger the number of simulations we use, the less noise we can observe. This can be understood in the information science as follow: because there are a finite number of paths in each MC simulation run, only partial information is realized. Making the right side of Eq.(7) as close as possible to the left side of the Eq.(7), with an optimized weights  , would give us the best fit. This WMC idea was actually first introduced for the variance reduction of MC simulation in the mathematical finance modeling by Avellaneda, et al. [8].  By borrowing this idea, we have been using WMC method to approximately calculate sensitivities before a semi-closed form solution was introduced.

We can extend the choices of Eq.(7) based on the above idea. Apart from those constraints used to generate the MC paths, we could add more constraints such that the simulation is guaranteed to reproduce the prices of  additional benchmark securities, whose prices are known from market data.  Then a simulation thus calibrated to benchmarks can price securities, that can not be directly observed from the market, such as value of FSCDO.

Given that the number of benchmark instruments  , the number of simulations, the system of equations (7) is severely underdetermined, admitting possibly an infinite number of solutions.  The question thus conmes as to which of the many solutions to choose.

Intuitively, it makes sense to choose a solution that is as close as possible, in some sense, to the standard  , while simultaneously matching the benchmark prices. This idea was sumarized and mathematically described by Shanon in 1948, which he used to measure the choice, uncertainty and information in a probability distribution [10].

In order to to define what we mean by “as close as possible,”  let

(8) 			 

where   is a convex function.   The search for a set of probabilities   to minimize  , subject to the constraints (3),  can be performed by introducing    Lagrange multipliers   and solving the Lagrangian dual problem, given by

(9)		  

There are many possible choices for  , one choice reads

(10)		 ,

which corresponds to the relative entropy, defined as the information theoretical entropy in information science.  

Note that Eq. (10) was first proposed by Shannon [10].  Briefly speaking, he proposed a constructive criterion for setting up probability distribution on the basis of partial knowledge.  A simple check would verify that Eq.(10) is maximum when  . For a physicist it is easy to spot the similarity between Eq. (10) and entropy in statistical mechanics. Actually Jaynes [11,12] indeed proved that this Shanon entropy actually bears the same idea of entropy in the statistical mechanics.  

It can be shown that the probability weights   which maximize (9) given a set   is given by

(11)			  

where   is the partition function, defined as

(12)		 . 

The minimum of (9) over   can be obtained by minimizing the objective function

(13)			 , 

Then the task of optimization come down to finding the appropriate vector of  such that we have a minimum  . Assuming the solution be  we can compute the path probabilities   as

(14)			 . 

A slight modification of the above procedure can be made if it is felt that an exact match to the benchmark prices is undesirable.  This situation may occur due to liquidity considerations, or other difficulties in obtaining accurate prices for the benchmark securities.  In this case, a small discrepancy between the input benchmark prices and those implied by the calibrated simulation is allowed.  This effectively means that we prefer calibrated path probabilities that are closer to the uniform   in exchange for slight mismatches in the benchmarks.  This shifts the relative importance of the benchmarks versus the relative entropy, placing more emphasis on entropy.

To allow this slight mismatch of benchmarks, we introduce a tolerance term into the objective function  , as

(15)		  

where   is an overall tolerance factor.  is an optional tolerance factor, which may in general by definition a function of the benchmark price  .

There exist many methods to minimize the objective functions of Eqs. (13) and (15).  The L-BFGS algorithm is employed in both the model and the test model used. In the test model, we use the same method as the one.

In the model the default choices of the parameters used in the optimization are shown in Table 1. The two most important parameters are the function tolerance and gradient tolerance. The gradient of the objective function is defined in Eqs.(15)

Note that the default values of the parameters, shown in the Table 1, have been tested and the results are outlined in the next section. These values are adequate for the test trade with CDX5 index pool as the underlying collateral pool. Any change to relax these parameter settings should be dealt with carefully and subject to testing. 

Reference:

https://finpricing.com/FinPricing-ProductBrochure.pdf

