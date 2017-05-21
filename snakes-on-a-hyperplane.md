# Snakes on a Hyperplane: Python Machine Learning in Production
* presenter: Jessica Lundin, ML Manager, Microsoft Research (healthcare)
* "Common Level" talk
* Input Space --> Feature Space
* https://notebooks.azure.com/LundinMachine

# What is machine learning?
* machine learning explores the study and construction of algorithms that can learn from and make predictions on data

**Examples:**
* Cat video classification
* Handwritten digit identification - Yann LeCun
* Lung cancer detection - Lundin's specialty

# Machine learning in production: practical tips
* Preproduction data --> Fit model --> Measure Preproduction Baseline Results
* Production data --> Apply model --> Measure Production Results + Compare to preproduction

# Visual Examples
(missing)
* Synthetic data- Production data is not going to be exactly like pre-production.
    * Beta users come from fellow peers at MS. Not a standard representation of our larger population.
    * Anything she trains on beta pool won't necessarily extend beyond that.
    * Common until you have production data to test on.
    * "Balanced dataset" in the example.
    * Production data was flipped on axis from preproduction data --> accuracy from 95% to 5% because feature space was different
* Unknown production distribution --> How do you handle it?
    * Retrain with non-linear algorithms.
        * e.g. Random Forest (overfitted, 100% accy)
        * e.g. Support Vector Machine w/ Radial Basis Function (93% accy)
    * Feature engineering to linearize features
        * One of the best ways to get high accuracy in the longer term is to be clever with your features
        * e.g. `x_1 * x_2`
    * Goal is to think about the simplest model that gives high accuracy.
        * Linear methods are more performant than ensembling methods.
        * Guarantees of convergence are generally going to be higher.
        * Linear models are interpretable.

# Model performance: unknown production distribution
* What if you have differences in distributions?
* Visualization (histograms, pairplots)
* Clustering- unsupervised methods on top of supervised methods
    * Visualize your clusters and look if your prod data produces its own clusters
* Kullback-Leibler (KL) divergence
    * Method to (missing)

# Model performance: unbalanced problems
* e.g. Fraud, which is very rare
    * Prediction might predict everything is of the same class
    * Previously looked at accuracy
    * Now look at precision and recall...
      * (missing)
      * Predicted positives = 0
      * Recall = 0 (missing)
* Oversampling
    * Can upsample rare class by adding noise.
    * Replace the sample back in. (It doesn't go away.)
    * 50/50 balanced data now.
    * Now can use statistic regression that gives a reasonable answer.
* Techniques for unbalanced problems
    * Cost-sensitive classification:
        * Rare-class upsampling with replacement
        * Importance weighting
        * Boosting- penalize problem where you incorrectly estimate
    * Treat it as an anomaly detection problem (one-class SVM)
        * e.g. as an unsupervised problem

# Hyperplanes
(missing diagram)
* No way to draw a straight line (Input Space)
* Transform into an infinite space
* "Kernel Trick"

# Machine learning in production: practical tips
* Logging
    * Timestamp, Instance IDs
    * Model run time- take a little IO hit to capture runtime so you can ensure the work is happening within its specified goals
    * Model results, performance metrics- useful to track drift
    * Model convergence errors- hopefully you'll set up a robust model ahead of time, but SK Learn only throws warnings if you have these types of errors. (linear models will reduce the likelihood of this.)
* Auditing
    * Manual process of digging into logs and data to resolve unexpected behavior
    * Build a cohesive story of what went long

# Machine Learning Resources
* Introduction to ML by Coursera by Andrew Ng - Matlab/Octave
* The ELements of Statistical Learning by Hastie, Tibshirani, Friedman - free ebook
* Kaggle Tutorials
* ML in Python
    * Scikit Learn
    * A ton of NN packages coming out
    * Rpy2: Python's R wrapper

# Microsoft Python resources
* Azure SDK
* Intro to Python Programming
* Python tools for Visual Studio
* Cognitive Toolkit (CNTK)
