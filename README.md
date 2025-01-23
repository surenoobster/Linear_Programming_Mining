# Linear_Programming_Mining

```
from pulp import LpProblem, LpVariable, LpMinimize, lpSum

# Create the LP problem
prob = LpProblem("Fuel_Optimization", LpMinimize)

# Variables
x = [LpVariable(f"x{i+1}", lowBound=0) for i in range(4)]  # Fuel purchased
f = [LpVariable(f"f{i+1}", lowBound=0) for i in range(4)]  # Fuel onboard

# Parameters
min_fuel = [23,8,19,25]
max_fuel = [33,19,33,33]
fuel_used = [12.1 , 2.0 , 9.5 , 13.0]
additional_burn_rate = [0.040, 0.005, 0.025, 0.045]
prices = [8200, 7500, 7700, 8900]

# Objective function: Minimize total cost
prob += lpSum(prices[i] * x[i] + additional_burn_rate[i] * (f[i] - min_fuel[i]) for i in range(4))

# Constraints
# Fuel inventory balance
prob += f[1] == f[0] + x[0] - fuel_used[0], "Delhi_to_Hyderabad"
prob += f[2] == f[1] + x[1] - fuel_used[1], "Hyderabad_to_Cochin"
prob += f[3] == f[2] + x[2] - fuel_used[2], "Cochin_to_Chennai"
prob += f[0] == f[3] + x[3] - fuel_used[3], "Chennai_to_Delhi"

# Fuel limits
for i in range(4):
    prob += f[i] >= min_fuel[i], f"Min_Fuel_Limit_{i+1}"
    prob += f[i] <= max_fuel[i], f"Max_Fuel_Limit_{i+1}"

# Solve the problem
prob.solve()

# Output the results
print("Optimal Fuel Purchase Plan:")
for i in range(4):
    print(f"City {i+1}: Purchase {x[i].varValue:.2f} (1,000 gallons)")
    
print("\nTotal Cost: ₹", prob.objective.value())

```
