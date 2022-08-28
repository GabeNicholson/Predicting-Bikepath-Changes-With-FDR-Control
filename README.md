# Predicting Bike path Changes While Limiting False Discoveries

A proof of concept attempt at correlating the residuals from OLS with time to make inferences on which bike paths changed over the course of two years. As far as I know, nothing like this has been tried before and it's validity/benefits with FDR control is discussed further in the code notebook. Because we are dealing with over 1.3 million samples and thousands of bike routes, the potential for false discoveries is substantial.


The notebook is split into two parts. The first is the **X** notebook that downloads and cleans the data. It also offers explanations for why certain data manipulations are made. For instance, deciding on discrete cutoff points for the time of day or which bike routes to include and which ones to discard. All of these decisions were made a-priori and were not data dependent. 

The second notebook **XXX** deals with the actual analysis. There are comments and explanations within that notebook which walk you through the code and the results.