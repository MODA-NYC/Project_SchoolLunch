# School Meal Reimbursements Project
Agency Partner: Department of Education

## Background and Scope

#### Providing free universal lunch to all NYC students

The NYC Department of Education (DOE) manages the largest school system in the US with over a million students. DOE is responsible for providing breakfast and lunch to NYC school students. NYC participates in the National School Lunch Program, run by the USDA. Traditionally this worked by giving a subsidy based on the income level of the individual student's family. Because this lead to stigmatization of students from low income families there has been a move towards more inclusive programs providing universal lunch to all students regardless of their family's income level. The Community Eligibility Provision (CEP) was created by the federal government in 2010. Under this program schools receive subsidies based on the overall makeup of the school (or group of schools). Participating schools are required to provide breakfast and lunch at no charge to all students. NYC participated in an initial phase of the program in 2014 with stand alone middle school. In 2017 NYC DOE made the bold decision to include all NYC schools in this program.

More information on NYC's initial involvement in CEP is available in a [Fiscal Brief](http://www.ibo.nyc.ny.us/iboreports/if-no-student-pays-cost-to-provide-free-lunch-for-all-of-new-york-citys-elementary-school-students.html) by the NYC Independent Budget Office.

#### Analytics Question
The amount of federal reimbursement for CEP depends on the percentage of identified students or students categorically eligible for free meals. This percentage is based on the student’s participation in other program such as SNAP and TANF. Schools can be enrolled as groups and there are no geographic or other constraints on how to group the schools. The subsidy parameters, and hence the reimbursement amount, change based on how the schools are grouped. The challenge is to find the optimal school groupings to maximize the reimbursement.


## Data
We worked with DOE to fully understand the problem outlined in the previous section along with all the rules and the process for obtaining reimbursements. Based on this information it was straightforward to identify the necessary data.

For each school the following data is needed:
* the number of identified students,
* the total number of students, 
* the projected number of breakfasts and lunches for the coming year.

The projected number of meals is based on meals served in previous years. Because most of these schools were not part of a universal lunch program prior to this, the meal projections will likely be different from the actual meals served. However because the school groupings need to be submitted before the meals are calculated, an exact number is not possible.

## Analysis
This problem falls in the category of combinatorial optimization. This is the same category as famous problems that are easy to state but hard to solve such as the traveling salesman problem and the knapsack problem. It is straightforward to solve for a handful of schools, but as the number of schools grows a direct or brute force approach quickly because impossible. With 1500 schools an exact result is out of reach, but we were able to try a few approximations.
We tried a number of approaches and found Monte Carlo approaches were the most elegant to implement and also gave the best results. Monte Carlo techniques have traditionally been used as a clever way to simulate a complicated probability distribution. In this case, we were only interested in the maximum value (highest reimbursement), so we used two well known derivations: stochastic hill climbing and simulated annealing.
We implemented these approaches in Python, using Pandas vectorized methods to reduce runtime. In addition we used the mulitprocessing package to do runs in parallel and choose the best of the lot.
There may be other methods giving as good or better results. Genetic algorithm implementations may also be a good candidate for this type of problem.

## Pilot
After much testing we ran the algorithm on schools new to the CEP program and provided DOE with the groupings. The groupings will be used in the 2017-2018 school year.

## Handoff
DOE will be able to use this algorithm in subsequent years to calculate the school groups. After the first year, schools have the option to stay in their current group or be regrouped. The algorithm can be adapted to address this.
