# Demystifying EEG Microstates
EEG Microstates allow for a sparse characterisation of spatial and temporal features of large scale brain network activity.
EEG Microstates, also nicknamed as “the atoms of thought”, are transient, patterned and quasi stable states of EEG.
- Transient: They exist only for a short time duration (Durations of microstates during
spontaneous task-free resting EEG on average are in the range of 70 to 125 milliseconds) - Patterned - Observable and noticeable
- Quasi Stable - Global topography is fixed, but strength might vary and polarity invert.

[Paper](https://ieeexplore.ieee.org/document/391164)<br />
[Tutorial Implementation](https://www.biorxiv.org/content/10.1101/289850v1)


# Analysis Methodologies 
<p align="center">
    <img width="854" alt="1" src="https://user-images.githubusercontent.com/44245211/137904136-d36ea876-4ac8-4d4d-8694-e1a029626b64.png">
</p>
- Consider this EEG time series with 32 electrodes sampled at every ‘x’ seconds. For this time series we have ‘x’ EEG states which are [32x1] vectors.
<p align="center">
    <img width="850" alt="2" src="https://user-images.githubusercontent.com/44245211/137904166-fec0e503-284a-4c17-8908-95c90a9e4e81.png">
</p>
- We now have a topographical map corresponding the EEG states.
<p align="center">
    <img width="324" alt="3" src="https://user-images.githubusercontent.com/44245211/137904173-e4e4db49-d7a4-4ca0-b0eb-7bf6d7374f1f.png">
</p>
- We start with choosing n (here = 4) random template maps. Then we use the k-means clustering algorithm to cluster or classify the EEG state maps into one of the n microstates.
<p align="center">
    <img width="855" alt="4" src="https://user-images.githubusercontent.com/44245211/137904176-c98e90ee-db60-4680-be03-4cb93a41d2be.png">
</p>
- After the EEG states have been assigned to a microstate, we see the EEG time series also classified to different (n) microstates. Here we see only one channel data as the above series only shows the GFP

### Global Field Power
- GFP (Global Field Power) - quantifies the amount of activity at each time point in the field considering the data from all recording electrodes simultaneously.
<p align="center">
    <img width="542" alt="5" src="https://user-images.githubusercontent.com/44245211/137904178-c1093303-c2fd-4a34-b638-ba174df0da7b.png">
</p>
- The measure of global field power (GFP) corresponds to the spatial standard deviation, and it quantifies the amount of activity at each time point in the field considering the data from all recording electrodes simultaneously resulting in a reference-independent descriptor of the potential field. We use L1 norm to calculate the GFP peaks.
<p align="center">
    <img width="322" alt="6" src="https://user-images.githubusercontent.com/44245211/137905150-6ef1a994-0f47-4ab4-8ae4-dc503c5b19ea.png">
</p>
~ Where,<br />
~ K - number of channels<br />
~ V(t) - EEG state represented by a (K x 1) vector

# Analysis Algorithm
{In most scenarios, number of EEG recording channels (Ns) >> number of microstates (Nµ) as with EEG microstates we are trying to represent an EEG time-series with the least possible number of states.}

- Let us consider a model with just 2 EEG recording channels (Ns = 2). Thus we have a 2-D plane that represents all possible electric potential values, and a single point corresponds to an instantaneous measurement. 
- For a classification model to be effective, it needs to have a minimum of 2 clusters, thus in this scenario we have 2 microstates. (Nµ = 2)

- Now, we initialize the two brain microstates as random vectors which are at unit distance from the origin. All points lying on the line going from the origin toward the microstate belong to the same microstate. 

- In mathematical terms the final aim for microstate analysis is,
<p align="center">
    <img width="168" alt="7" src="https://user-images.githubusercontent.com/44245211/137906292-42b6e13f-edcd-4cad-a51a-1ff9e633d0cd.png">
</p>
~ Where,<br />
~ Vt = (Ns x 1) vector consisting of the scalp electric potential measurements <br />
~ Γk = is the normalised Ns x 1 vector representing the kth microstate <br />
~ akt = the kth microstate intensity at time instant t. 

- Moreover, every EEG state can only be represented by just one microstate i.e. in order to allow for non-overlapping microstates at each time instant t, all akt must be zero except for one. Therefore, at each time instant, the summation reduces to a single nonzero term, corresponding to a single active microstate.

- Now, the above mentioned equation will not be able fit perfectly especially when (Ns >> Nµ) or when the EEG time series is long ie. we have many EEG states. Thus we add a term for error.
<p align="center">
    <img width="211" alt="8" src="https://user-images.githubusercontent.com/44245211/137906296-3d56a344-947d-4332-b2f8-105552844795.png">
</p>
- The goal will be find microstates such that <br />
1. Every EEG state will belong to only one microstate <br />
2. The total error in the system will be minimum. The orthogonal squared distance between each measurement vector and microstate is computed. The higher the distance the higher the error.
<p align="center">
    <img width="233" alt="9" src="https://user-images.githubusercontent.com/44245211/137906300-de37fce7-3fee-4666-9242-4093e2313f1f.png">
</p>
- Let us assume we now have 32 electrodes and choose to classify them into 4 microstates. <br />
1. We have a 32 dimension space, with every point a possible EEG state. Dimension of the EEG state here is 32x1.<br />
2. We randomly construct random vectors in the above mentioned space of unit length. The dimension of the microstate is 32x1.<br />
3. We now use k-means algorithm which gives the 4 most optimal microstates; and classifies all EEG states into one of those microstates with the orthogonal squared distance (error) less than the threshold error which was set by the user.

# Directory Structure
- ```src``` folder contains the source code. 
- ```img``` contains all the images to test the code on.
 
# Running the code
Any changes which need to be made, must be made in the code block under the heading "Main" only
1. Run the code which launches the GUI based environnment.
2. Draw a rectagle around the region to be kept using right mouse button.
3. Next you can mark refinment storkes - Mark with these keys 0 (background) and 1 (foreground).
