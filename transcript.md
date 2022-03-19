1. First of all, I would like to thank you for allowing me to speak in front of you today. My name is Luciano Melodia and I will talk about homological analysis of time series data from power plants.

2. I work at the Friedrich-Alexander-University Erlangen-Nuremberg and I am a PhD student at the Department of Evolutionary Data Management. Our project that I am presenting today is a collaboration of our Chair of Computer Science 6, i6 for short, and Siemens Energy, who kindly provided us with the power plant data.

3. Let me start with a brief description of the upcoming content. Sensor data streams are named according to a standardized designation system. Unfortunately, this is not consistently applied by power plant operators, so engineers today manually label sensor data streams when they want to analyze power plant data. This is the reason for our classifier, which aims to automate this task. I will also briefly introduce the structure of the technical argument. How can homology groups be used for feature extraction, and why do we succeed in training our classifier with representations of homology groups of dimension zero and one? At this point I must assume prior knowledge of persistent homology, but I am happy to clarify questions about it afterwards. We assume that the time series lie on a 1-manifold, but since this is rather boring for homological analysis, we use an embedding we call sliding-window, which depends on two parameters, M and tau, to represent the time series as a curve within or densely within a hypertorus. Its homology groups are informative and useful for us. Then I will show the architecture of the neural network we have used and present the experimental results.

4. Let's start with the classification of power plant sensor data.

5. Here you can see the components of the labeling system. First, the overall plant is described by a letter or a number. Following the same procedure, the functional units are coded and described by an optional number, three letters, and two more numbers that enumerate the functional units. They form a coherent group of components within a power plant . Aggregates are smaller subsets of functional units. Thus, a hierarchical structure is used to encode the architecture of a power plant in the designations. Finally, two letters and two numbers are used to encode the actual unit to which the sensor is attached. Let me explain this with an example. Here, the entire unit is coded with the number one. The functional unit is coded as main group 2L, which means the second steam, water and gas loop. A is the feedwater system and C is the feedwater pump. Finally, the numbers 0 and 3 describe that it is the third feedwater pump of the power plant. The same procedure is followed with the aggregates and operating equipment.

6. Before we come to the theoretical foundations of our approach, I would like to remind you of the so-called manifold assumption. This states that we can start from a one-dimensional, possibly smooth manifold underlying our set of points forming the time series for a sensor. Of course, this manifold would be homeomorphic to the real axis and thus relatively boring in terms of homological investigations. Therefore, we embed the points in a higher dimensional object whose homological properties encode certain properties of the manifold on which we have conjectured the data.

7. in equation one, we can see how the vector for a point looks as a function of two parameters, the step size Tau and the dimension of the embedding space M+1. M times Tau is then referred to as the window size. The set of points associated with the time series T is then defined as can be seen in equation two. Now the periodicity of the function f(T) is directly related to the roundness of the embedding of the set of points in some Euclidean space of dimension M+1, where a period is defined as having the same function image for two different values of the domain. The number of underlying harmonic components provides information about a suitable embedding dimension, which should be greater than 2N. A similar relationship exists for the number of frequencies, which corresponds to the number of copies of spheres as factor spaces for the hypertorus. This means that we can describe basic properties of the function of our time series by topological and geometrical properties of a hypertorus, which we can take as a manifold under the embedded set of points.

8. I briefly illustrate here the sliding window embedding of a function f(T) in the corresponding point set, which is a subset of R M+1.

9. Now I would like to discuss the homological properties of the torus. These allow us to fully describe the hypertorus by only the zeroth and first homology groups, so that we can use these as features for our classifiers without having to use higher homology groups, and still perform sufficient feature extraction. Let T2 be isomorphic to S1 times S1 and let Zp be a field, i.e., Z modulo (pZ), where (pZ) is a maximal prime ideal and p is a prime number. We exploit the fact that the zeroth homology group of S1 over the field Zp is equal to the first homology group of S1, namely the field Zp itself. Thus, the ith homology group of S1 is zero for all i > 1, since homology groups with dimension larger than that of the manifold itself are trivial. For the first homology group of the second torus, we obtain two copies of the fundamental field. We obtain the field itself for the homology group of dimension two. Again, all higher homology groups are trivial.

10. The homology groups of a sphere are torsion-free. Since we are working in a field of coefficients, we can apply Künneth's formula since all modules over a field are free. Therefore, we can generalize for the n-dimensional hypertorus that it is a product space of n copies of the sphere. Its k-th homology group is then given as the direct sum over all indices i-one to i-R, whose sum equals k. Under the direct sum we have the tensor product of the corresponding i-th homology groups of the factor spaces. Note that nothing happens in the equation unless the indices are 0 or 1, since all higher homology groups of the sphere are zero. This gives the expression of the kth homology group of the torus as Zp to the power of n choose k. We have now established a connection between the dimension of the torus and the homology groups.

11. let us briefly recall the definition of the kth Betti number, which appear in the persistence diagrams as persistent Betti numbers along the filtration, as the rank of the kth homology group. The Betti numbers for the n-torus can be obtained from this table for increasing n. We note that the zeroth Betti number always remains the same since the torus is path connected. Nevertheless, the properties of the zeroth homology group are important to us because the persistence diagrams in the zeroth dimension depend on the distance function chosen to construct the filtration. Thus, the zeroth persistent homology group gives us information about local connectedness. The first homology group identifies the n-dimensional torus, since it is unique for each n and corresponds to its dimension. Therefore, we can identify the manifold only by this Betti number.

12. In the next section, I describe our experimental setup, which has empirically proven to be the best fit for our time series.

13. Our neural network consists of three subnetworks. The very first component with batch normalization is used to process a sample of the raw time series data. The second and third subnetworks without batch normalization are used to process the zero- and one-dimensional Betti curves, a special representation of the zeroth and first persistent homology groups lying in a Hilbert space. This representation allows us to compute statistics for the given points. We combine these inputs additively before further processing the data through LSTM layers. All layers are implemented with residual connections.

14. Let us now talk about the results.

15. They are promising. We wanted to predict the identifiers with the numbering of each component, which means that we associate the signal with the entity of the sensor. We tried to predict the overall system alone, the overall system and the functional units, the overall system, the functional units and the aggregate, and the complete identifier. This table can be found in our publication. For the prediction of the total system alone, the neural network without residual connections performed better. We could not determine the reason for these results.

16. The best classification results are about 64% accuracy for the full identifiers, about 65% up to the aggregate, 73% up to the feature level, and 83% for the whole system. For all experiments, the precision is much larger than the recall. Thus, the precision of our classifier is relatively large compared to its average completeness per class. We have shown that residual connections improve the classification results for all labels, except for the assignment of the overall system. In addition, we showed that using Betti curves as features for a classifier was superior to classifying time series alone for each variant of power plant label prediction.

17. So what have we learned?

18. Other experiments conducted by some of our students show that the OR entity achieves the highest accuracy in predicting power plant identifications in all models tested, followed by A, F, and OS. This is promising since we have already demonstrated 83% accuracy for OS. Thus, our model may be able to predict the isolated parts of the identifiers with even higher accuracy.

19. Since the signal is embedded in a torus, one could construct neural network layers operating on a given Lie group composed as a product space of spheres and Euclidean spaces. By applying parallel transport, one could work exclusively on such manifolds during training and tailor them specifically to the data set. The required smooth manifold can be
be derived from the persistence graph.

20. Further experiments should be performed without using the corresponding numbers of aggregates and functional units. This would lead to much higher accuracy, as we suspect, and would be suitable for practical application.

I would like to end the presentation with a reference to my other work and to other sources I have used for this presentation.

If I have piqued your interest, please do not hesitate to contact me. Also, don't forget to visit and star our repository! Thank you very much for your attention!