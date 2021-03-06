### [The Welfare Effects of Encouraging Rural-Urban Migration](http://www.waugheconomics.com/uploads/2/2/5/6/22563786/LMW.pdf)

---
This repository contains code to reproduce results from the paper ["The Welfare Effects of Encouraging Rural-Urban Migration"](http://www.waugheconomics.com/uploads/2/2/5/6/22563786/LMW.pdf). It also includes replication files (empirical and quantitative results) for the paper ["Underinvestment in a Profitable
Technology: The Case of Seasonal Migration in Bangladesh"](https://onlinelibrary.wiley.com/doi/abs/10.3982/ECTA10489) that were downloaded from Econometrica's code repository and obtained from the authors. I will provide support for the former, but not for the later.

**Complete explanations of the repository are currently under construction.**

**Software Requirements:** Outside of plotting (and the data analysis of the field experiment) all of this code is in MATLAB with the Parallel Computing Toolbox (for computation of the model) and the Global Optimization Toolbox (for calibration).

With access to 12 cores processors (Xeon E5-2690) the model (with a dense productivity and asset grid) is solved in about 100 seconds. Simple modifications to speed up computation are to reduce the productivity grid, which out loosing much in terms computing equilibrium outcomes.

#### Basic Calls
---
**Compute Model** The most basic call to start inside the [``\revision_code\calibration``](https://github.com/mwaugh0328/welfare_migration/tree/master/revision_code/calibration) folder. From there to generate outcomes from the model and the partial equilibrium welfare numbers is here:

```
>> load('calibration_final.mat')

>> analyze_outcomes_prefshock(exp(new_val), 1)
```
Then it should compute everything and then spit out the moments. In ``analyze_outcomes_prefshock.m`` you can see each step (i) value function iteration (ii) simulation (iii) implementation of experiment and (iv) collect results. The results should mimic (or come very close) to those in Table 2, 6, and 8 in the January 2020 version of the paper.

---
**Compute GE Counterfactual** Start inside the [``\revision_code\ge_counterfactual``](https://github.com/mwaugh0328/welfare_migration/tree/master/revision_code/ge_counterfactual) folder. Then call
```
>> eq_compute
```
which will do everything: Infer wages-per-efficiency units from the calibrated model, compute welfare holding wages fixed, compute welfare with wages changing in equilibrium. This should replicate the results in Table 8. Notes describing more detail about the GE computation are [here](https://github.com/mwaugh0328/welfare_migration/blob/master/notes_ge_version/notes_GE_analysis.pdf)

---

**Calibrate the Model** The calibration routine is implemented by starting inside the [``\revision_code\calibration``](https://github.com/mwaugh0328/welfare_migration/tree/master/revision_code/calibration)
```
>> calibrate_wrap_tight
```
And then within it you can see how it works. It calls ``compute_outcomes_prefshock.m`` which is similar to the ``analyze...`` file above but is optimized for the calibration routine.  The key to get this thing to fit was using the ``ga`` solver which is essentially a search of the entire parameter space in a smart way. Alternative approaches with different minimizers (``patternsearch`` ``fminsearch`` (with and without random start) and some ``NAG`` routines) are in the ``graveyard`` folder.
