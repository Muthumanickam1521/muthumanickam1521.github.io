---
title: "Demystifying Ensemble Learning Techniques"
description: ""
pubDate: 2024-06-14
heroImage: "/blog2_image1.png"
tags: ["Ensemble Learning", "Machine Learning 🤖", "Algorithms 👾"]
---

#### Who am I?
I’m a post-graduate in data science and currently about to complete a corporate internship. I’m having a good learning at my company and hoping to learn more.

#### Motivation
Writing about something that you learnt is more related to how much you have understood. I wish to begin writing on technical concepts and have a thought to develop this as a habit. Here comes, I get started with my first ever blog. At end of the day, it is all about what do you take home (learning).

To keep things simple and to get started, I write a simple program that runs in python programming language. Recently I got the motivation to learn Object Oriented Programming (OOPs) implementation of data science techniques. One may need OOPs to get structured and modular approach towards building Machine Learning (ML) models, though it is not necessary for industry data professionals.

#### Importance of Ensemble Learning
Real-world data may often have complex data-pattern and requires multiple-models to obtain decent predictive accuracy. Here comes the use of ensemble of two or more models.

Types (popular):

* Bagging: A method that creates multiple datasets from the source dataset using a technique called bootstrap resampling. A homogeneous estimator is trained multiple times with bootstrapped samples. The predictions are obtained and combined together at the end. In regression problem, averaging all predictions gives final predictions. In the case of a classification problem, voting is taken to combine them.

![image2](/blog2_image2.png)

* Voting: A technique that combines the results of multiple base learners (homogeneous or heterogeneous). The entire dataset is passed into the learners unlike bagging where re-sampling is performed. The final predictions are curated from these estimators based on majority of votes. Example: 10 heterogeneous models are trained on a chemical dataset to predict physical property of chemical compounds; they produce 10 different predictions. The final predictions are derived in a similar manner to how bagging ensemble learning produces results. The primary differences between bagging and voting methods lie in the choice of using same or different estimators for training with multiple samples and whether sample replacement is permitted or not. In voting, sampling is not done. On the other hand, it is allowed in bagging ensemble learning technique.

![image3](/blog2_image3.png)

* Stacking: A learning way that uses meta-learner to learn from the predictions of multiple base learners. The given number of estimators produce their predictions individually. Stacking use a meta-learner to learn predictions of all base learners. This method yields more robust results compared to other methods.

![image3](/blog2_image4.png)


#### Implementation
We create a simple class in python to get predictions using ensemble learning. Since it is our first version, we make things simple and less functional. In future, more functionalities can be included depending upon the problem.

The class encapsulates many methods (functions) and is ready for multi-functional uses. There are 8 regression model in total from DecisionTreeRegressor to LGBMRegressor in this blog. The scikit-learn has easy-to-use implementation of all these methods. Please refer from here.

```python3
# utils.py
class EnsemblePredictor:
    """
    Provides predictions of given dataset using ensemble techniques
    """

    def __init__(self, data, target_columns_name, model_names, ensemble_strategy='bag'):
        """
        Initialize class arguments
        """

        self.data = data
        self.target_columns_name = target_columns_name
        self.model_names = model_names
        self.ensemble_strategy = ensemble_strategy

    def split_data(self):
        """
        Split the given data using K-fold method
        """

        kf = KFold(shuffle=True, random_state=33)
        return kf.split(self.data)
            
    def fit_models(self, use='ensemble'):
        """
        Find the best fold based on RMSE and train base learners
        """

        pool = {'decision_tree': DecisionTreeRegressor(),
                'random_forest': RandomForestRegressor(n_jobs=4),
                'svr': LinearSVR(dual='auto'),
                'ridge': Ridge(),
                'lasso': Lasso(),
                'elasticnet': ElasticNet(),
                'gradient_boosting': GradientBoostingRegressor(),
                'lightgbm': LGBMRegressor(n_jobs=4, verbose=-1)
                }
        min_rmse = 0.0
        folded_data, trained_pool = list(), list()     
        splits = self.split_data()
        model_names = self.model_names

        try:
            if model_names.lower() == 'all':
                models_names = pool
        except AttributeError:
            pass

        target = self.target_columns_name
        y, X = data[target], data.drop(target, axis=1)

        for fold_idx, (train_index, test_index) in enumerate(splits):
            X_train, X_test = X.iloc[train_index], X.iloc[test_index]
            y_train, y_test = y.iloc[train_index], y.iloc[test_index]
    
            gbm = LGBMRegressor(n_jobs=4, force_row_wise=True, verbose=-1, random_state=2)
            gbm.fit(X_train, y_train)

            y_pred_test = gbm.predict(X_test)
            rmse = round(root_mean_squared_error(y_test, y_pred_test), 2)
            folded_data.append((fold_idx, X_train, X_test, y_train, y_test))

            if fold_idx == 0:
                min_rmse = rmse
                best_fold_idx = fold_idx

            if rmse < min_rmse:
                min_rmse = rmse
                best_fold_idx = fold_idx

        _, X_train, X_test, y_train, y_test = folded_data[best_fold_idx]
        
        if use == 'standalone':
            for model_name in model_names:
                model = pool.get(model_name)
                model.fit(X_train, y_train)  

                tuple_ = (model_name, model)
                trained_pool.append(tuple_)

                y_pred_test = model.predict(X_test)
                rmse = round(root_mean_squared_error(y_test, y_pred_test), 2)
                print(f'Base learner - {model_name}: {rmse}')
            return (X_train, X_test, y_train, y_test), trained_pool
        else:
            return (X_train, X_test, y_train, y_test), pool
 
    def ensemble_models(self):
        """
        Fit an ensemble model based on argument ensemble_strategy
        """

        (X_train, X_test, y_train, y_test), pool = self.fit_models()
        models = [(str_, pool[str_]) for str_ in pool.keys()]

        if self.ensemble_strategy == 'vote':
            custom_model = VotingRegressor(models)
        elif self.ensemble_strategy == 'stack':
            custom_model = StackingRegressor(models, final_estimator=LGBMRegressor(n_jobs=4, verbose=-1, force_row_wise=True, random_state=33))
        elif self.ensemble_strategy == 'bag':
            custom_model = BaggingRegressor(estimator=LGBMRegressor(n_jobs=4, verbose=-1, force_row_wise=True), n_estimators=100, n_jobs=3, random_state=33)
        
        custom_model.fit(X_train, y_train)
        y_pred_test = custom_model.predict(X_test)
        rmse = round(root_mean_squared_error(y_test, y_pred_test), 2)
        print(f'RMSE - {self.ensemble_strategy}: {rmse}')
        return y_pred_test, custom_model
    
    def predict_new_data(self, new_X):
        """
        Obtain predictions of unseen data with the trained-model
        """

        _, custom_model = self.ensemble_models()
        preds = custom_model.predict(new_X)
        print(preds)
        return preds
```

Only fit_models method takes use parameter that has [standalone / ensemble] options. ‘ensemble’ is the default value. It is helpful if one needs to use base model alone. If ‘standalone ’is assigned to it, the ensemble learning does not happen. Instead, your data gets trained with the base models and trained model objects are return as result. This method uses iteration to find the optimal data splits based on RMSE score.

Example: We use 5 split k-folds by default. Let’s say 3rd data split gives lower RMSE value compared others. Then we use that data split for building model.

The class EnsemblePredictor takes multiple arguments:

The class EnsemblePredictor takes multiple arguments:

> data → a pandas data frame (features + target) [NOTE: data has to be pre-processed to work bug-free] 
>
> target_column_name → name of target column
>
> model_names → a list containing names of models (‘all’ / any combination from [‘decision_tree’, random_forest’, ‘svr’, ‘ridge’, ‘elasticnet’, ‘lasso’, ‘lightgbm’, ‘gradient_boosting’])
>
> ensemble_strategy → Ensemble technique to use (‘bag’ / ‘vote’ / ‘stack’)

![image5.png](/blog2_image5.png)

To use the class, just assign values to class arguments and call ensemble_models() or predict_new_data() method directly as shown the above picture.

One can also create multiple objects of ensemble models with different strategies and model combinations. The different values of random state parameter of models as well as splitting algorithm may yield different results. There is a room for experimentation in order to achieve considerably good results.

Thank you for reading patiently!

