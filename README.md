
# Traffic Simulation

## Introduction
This project models the flow of traffic on the four-way intersection at Jingming Street in Taiwan. This location was chosen as it is a popular location through which most Minervans transit to get anywhere in the city and to get a quick snack at the 7-Eleven or OKMart located at the intersection. The simulation aims to identify the optimal traffic light strategy for reducing congestion and improving traffic flow.

## Data Collection
| Road | Number of cars passing through the intersection | Time (s) | Avergae number of cars per second | Green Light | Red Light|
| -------- | -------- | -------- | -------- | -------- | -------- |
| North Bound | 93 | 300 | 0.31 | 30s | 60s |
| South Bound | 62 | 300 | 0.21 | 30s | 60s |
| East Bound | 240 | 300 | 0.80 | 60s | 30s |
| West Bound | 215 | 300 | 0.72 | 60s | 30s |
<p style="text-align: center;"><strong> Table 1.</strong><em> Summary of data collected at the intersection</em></p>

Data was collected after observing the flow of traffic for around 30 minutes around 3 pm, which saw a high amount of traffic. The proportion of cars turning right at the intersection was around 2 in every 5 cars. The data collected includes the number of cars passing through the intersection, time, and average number of cars per second for each road (North Bound, South Bound, East Bound, and West Bound).

## Model
The Nagel & Schreckenberg (1992) model was used as a starting point for the rules of the system. Additional rules were added to model the four-way intersection with four different roads, with the biggest change to the original model representing a change from a 1-d cellular automaton to a 2-d cellular automaton, with an implementation of traffic lights and right turns.

<table>
  <tr>
    <th>Type</th>
    <th>Rule</th>
  </tr>
  <tr>
    <td colspan="2" align="center"><b>Original Rules</b></td>
  </tr>
  <tr>
    <td>Accelerate</td>
    <td>A vehicle accelerates by 1 if its current velocity is lower than the maximum speed; this only happens if the new...</td>
  </tr>
  <tr>
    <td>Decelerate</td>
    <td>A vehicle decelerates if its current velocity is equal to or greater than the distance to the next car. The velocity...</td>
  </tr>
  <tr>
    <td>Slow down</td>
    <td>A vehicle randomly slows down by 1 with a probability of p_slow</td>
  </tr>
  <tr>
    <td>Movement</td>
    <td>A vehicle moves v cells every time step, where v is the velocity</td>
  </tr>
  <tr>
    <td colspan="2" align="center"><b>Additional Rules</b></td>
  </tr>
  <tr>
    <td>Cells</td>
    <td>The model uses a 2-dimensional cellular automaton, where each cell is either a -1 (empty) or v, representing the...</td>
  </tr>
  <tr>
    <td>Boundary</td>
    <td>Absorbing boundary condition rather than periodic unlike in Nagel & Schreckenberg so cars disappear and leave...</td>
  </tr>
  <tr>
    <td>Traffic stop 1</td>
    <td>Vehicles can only go straight if the traffic light is green</td>
  </tr>
  <tr>
    <td>Traffic stop 2</td>
    <td>The traffic light for the East and West bound roads are synchronized and the inverse of the South and North...</td>
  </tr>
  <tr>
    <td>Traffic stop 3</td>
    <td>Vehicles slow down and stop exactly at the traffic intersection for a red light</td>
  </tr>
  <tr>
    <td>Turn</td>
    <td>Vehicles make a right turn with a turning probability, and can only do so on a green light.</td>
  </tr>
</table>
<p style="text-align: center;"><strong> Table 2.</strong><em> Summary of rules</em></p>

Parameters in the model include the probability of slowing down, traffic light times, probability of right turns, and arrival rates for the cars.

## Analysis
Theoretical Analysis
The theoretical analysis in this model compared the theoretical values from the Mean Field Analysis of cellular automata. A simplification was made such that the average density and average flow of traffic were not calculated for the whole model (all four roads), rather, just for one lane (West to East road).
![mfa 0](https://user-images.githubusercontent.com/84438477/235646852-cc20a387-39d1-4df7-8eff-83801924b9ca.png)
<p style="text-align: center;"><strong> Figure 1.</strong><em> Traffic flow against car densities for p_slow = 0</em></p>

![mfa 0 25](https://user-images.githubusercontent.com/84438477/235647122-02908ef3-012e-4b77-9f1d-32792095fea4.png)
<p style="text-align: center;"><strong> Figure 2.</strong><em> Traffic flow against car densities for p_slow = 0.25</em></p>

![mfa 0 5](https://user-images.githubusercontent.com/84438477/235647211-934e4130-f8b7-46c6-93b7-ee6568ebf5f4.png)
<p style="text-align: center;"><strong> Figure 3.</strong><em> Traffic flow against car densities for p_slow = 0.5</em></p>

## Empirical Analysis
The empirical analysis to figure out a good strategy for the traffic lights was done, which entailed the effect of various traffic light times on the average flow of traffic. Three different traffic light strategies were explored, including the original strategy (based on real-life signals), the reversed strategy, and the third strategy based on the proportion of traffic between the East-West roads and the North-South roads.

![hist](https://user-images.githubusercontent.com/84438477/235647254-18aa4692-a31c-465f-a429-cf74074fcfd8.png)
<p style="text-align: center;"><strong> Figure 1.</strong><em> Histogram of different traffic light startegies and their effect on traffic flow</em></p>

## Conclusions
The mean traffic flow is highest with the third strategy, which reflects that there is a potential improvement to be made in the real-life traffic light strategy. If traffic planners can dynamically calculate the rate of traffic between the roads and then use that to determine the traffic light times in a manner which reflects the proportion of rates between the two sets of roads, then the average flow would likely increase.

However, one limitation of this empirical analysis is that the mean of the third strategy lies within the confidence interval of the original strategy, which means that it is possible the true population mean of the original and current strategy is as good as or even better than the suggested new strategy based on the car arrival rates.

## References
Nagel, K., & Schreckenberg, M. (1992). A cellular automaton model for freeway traffic. Journal de physique I, 2(12), 2221-2229.
Minerva University. (2023). CS166 Session 9. Traffic MFA Workbook
