
# Mixing solute problem

A 1500 gallon tank initially contains 600 gallons of water with 5 lbs of salt dissolved in it. Water enters the tank at a rate of 9 gal/hr and the water entering the tank has a salt concentration of $\frac{1}{5}(1 + cos(t))$ lbs/gal. If a well mixed solution leaves the tank at a rate of 6 gal/hr:

- how long does it take for the tank to overflow?
- how much salt (amount in lbs) is in the tank when it overflows?


## Step 1: Calculate time to overflow

In this step we will leave the calculation of the mass of salt aside. The goal is to solve the first question first since we have all the information readily available in the statement of the problem. Thinking about the second question or trying to solve both at the same time will make this problem more difficult than it is. I good way of thinking about this problem is as an accounting balance, where we have inflows (inflow rate), outflows (outflow rate), and a checking account (tank level).


```python
# First step: Calculate tank level and time of overflow using a for loop
import math

# Initial parameters
tank_capacity = 1500 # gallons
tank_level = 600     # gallons Initial tank_level
inflow_rate = 9      # gal/hr
outflow_rate = 6     # gal/hr

# Compute tank volume and salt mass at time t until tank is full
counter_hours = 0
while tank_level < tank_capacity:
    tank_level = tank_level + inflow_rate - outflow_rate   
    counter_hours += 1
    
print('Hours:',hours)
print('Tank level:',tank_level)

```

    Hours: 300
    Tank level: 1500


## Step 2: Add the calculation of the amount of salt

Now that we understand the problem in simple terms and we were able to implement it in Python is time to add the computation of the total salt at each time step. In this step is important to realize that concentration is amount of salt per unit volume of water, in this case gallons of water. Following the same reasoning for the first part of the problem, we need to calculate the balance of salt inside the tank, which is the result of the salt inflows and outflows plus whatever salt we had at the beginning. The initial amount of salt in the tank is analogous to the initial water in the tank or a seed money on a new bank account. So, to solve the problem we need:

- the inflow rate of water with salt
- the salt concentration of the inflow rate
- the outflow rate of water with salt
- the salt concentration of the outflow rate (**we need to calcualte this**)

From the statement we have the first 3 pieces of information, but we lack the last one. Since concetration is mass of salt per unit volume of water, we just need to divide the total amount of salt over the current volume of water in the tank. So at the beginning we have 5 lbs/600 gallons = 0.0083 lbs/gal, which will be the salt concentration of the outflow during the first hour. Becasue the amount of water and salt in the tank change every hour, we need to include these computations in each iteration to update the salt concentration of the outflow.



```python
import math

# Initial parameters
tank_capacity = 1500 # gallons
tank_level = 600    # gallons
salt_mass = 5        # lbs
inflow_rate = 9      # gal/hr
outflow_rate = 6     # gal/hr
counter_hours = 0

# Compute tank volume and salt mass at time t until tank is full
while tank_level < tank_capacity:
   
    salt_inflow = 1/5*(1+math.cos(counter_hours)) * inflow_rate
    salt_outflow = salt_mass/tank_level * outflow_rate
    salt_mass = salt_mass + salt_inflow - salt_outflow
    tank_level = tank_level + inflow_rate - outflow_rate
    counter_hours += 1
    
print('Hours:',hours)
print('Tank level:',tank_level)
print('Mass of salt when overflowing:',round(salt_mass),'lbs')
        
```

    Hours: 300
    Tank level: 1500
    Mass of salt when overflowing: 280 lbs


## Using Numpy

An alternative solution using numpy, pre-allocating arrays using NaNs, a for loop, and storing variables for posterior plotting. Of course, you can also keep track of the variables by using regular Python lists in combination with the append method.


```python
import numpy as np

# Initial parameters
period = 1000        # Large number of hours to ensurehours
tank_level = np.ones(period)*np.nan # Pre-allocate array with NaNs
salt_mass = np.ones(period)*np.nan  # Pre-allocate array with NaNs
tank_level[0] = 600   # gallons
salt_mass[0] = 5        # lbs
tank_capacity = 1500 # gallons
inflow_rate = 9      # gal/hr
outflow_rate = 6     # gal/hr

# Compute tank volume and salt mass at time t until tank is full
for t in range(1,period):
    
    # The salt concentration will be computed using the tank level of the previous hour
    salt_inflow = 1/5*(1+np.cos(t)) * inflow_rate          # lbs/gal ranges between 0 and 0.4
    salt_outflow = salt_mass[t-1]/tank_level[t-1] * outflow_rate
    salt_mass[t] = salt_mass[t-1] + salt_inflow - salt_outflow
    
    # Now we can update the tank level
    tank_level[t] = tank_level[t-1] + inflow_rate - outflow_rate   # volume of the tank
   
    # Added greater than just in case the tank level does not exactly match the tank capacity value
    # For instance, if we set the condition to '==' and the tank_level changes from 1499 to 1501
    # between two iteration steps, then the loop will never stop.
    if tank_level[t] >= tank_capacity:
        print(t, 'hours')
        print(np.round(salt_mass[t]),'lbs of salt')
        break
```

    300 hours
    280.0 lbs of salt


## Plot charts


```python
import matplotlib.pyplot as plt

plt.figure()
plt.plot(range(period),tank_level)
plt.xlabel('Hours')
plt.ylabel('Tank level (gallons)')
plt.show()

plt.figure()
plt.plot(range(period),salt_mass)
plt.xlabel('Hours')
plt.ylabel('Salt mass in the tank (lbs)')
plt.show()

plt.figure()
# Plot every 5 values to clearly see the curve in the figure
plt.plot(range(0,period,5), 1/5*(1+np.cos(range(0,period,5))))
plt.xlabel('Hours')
plt.ylabel('Inflow sal concentration (lbs/gal)')
plt.show()
```


![png](output_8_0.png)



![png](output_8_1.png)



![png](output_8_2.png)

