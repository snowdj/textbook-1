---
interact_link: notebooks/Chapter_04/01_Joint_Distributions.ipynb
title: '4.1 Joint Distributions'
previouschapter:
  url: chapters/Chapter_04/00_Relations_Between_Variables
  title: 'Chapter 4: Relations Between Variables'
nextchapter:
  url: chapters/Chapter_04/02_Marginal_Distributions
  title: '4.2 Marginal Distributions'
---

## Joint Distributions ##

Suppose $X$ and $Y$ are two random variables defined on the same outcome space. We will use the notation $P(X = x, Y = y)$ for the probability that $X$ has the value $x$ and $Y$ has the value $y$. 

The *joint distribution of $X$ and $Y$* consists of all the probabilities $P(X=x, Y=y)$ where $(x, y)$ ranges over all the possible values of $(X, Y)$.

#### Example ####
In three tosses of a coin, let $X$ be the number of heads in the first two tosses and $Y$ the number of heads in the last two tosses. Then 

$$
P(X = 0, Y = 2) = 0 = P(X = 2, Y = 0)
$$

$$
P(X = 1, Y = 1) = P(\text{THT or HTH}) = \frac{2}{8}
$$

All the other probabilities are $1/8$, as you can see by examining the six remaining outcomes of three tosses. For example,

$$
P(X = 1, Y = 2) = P(\text{THH}) = \frac{1}{8}
$$

The constraints on $x$ and $y$ are that each must be in the range $\{0, 1, 2\}$ and $\vert x - y \vert < 2$. Let's define a function that takes $x$ and $y$ as its arguments and returns $P(X = x, Y = y)$.


{:.input_area}
```python
def joint_probability(x, y):
    if x == 1 & y == 1:
        return 2/8
    elif abs(x - y) < 2:
        return 1/8
    else:
        return 0
```

The `prob140` library contains Table methods for displaying the joint distribution of two random variables. As a first step, you need the possible values of each of the two variables. In our example, both have values $\{0, 1, 2\}$ and so the same list or array will serve for both.


{:.input_area}
```python
k = np.arange(3)
```

To construct a joint distribution object, we have to first construct a table of all possible pairs of values and probabilities. The call is:

`Table().values(variable_name_1, values_1, variable_name_2, values_2).probability_function(function_name)`

where `function_name` is a function that takes $x$ and $y$ as arguments and returns $P(X = x, Y = y)$.


{:.input_area}
```python
joint_table = Table().values('X', k, 'Y', k).probability_function(joint_probability)
joint_table
```




<div markdown="0">
<table border="1" class="dataframe">
    <thead>
        <tr>
            <th>X</th> <th>Y</th> <th>Probability</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0   </td> <td>0   </td> <td>0.125      </td>
        </tr>
    </tbody>
        <tr>
            <td>0   </td> <td>1   </td> <td>0.125      </td>
        </tr>
    </tbody>
        <tr>
            <td>0   </td> <td>2   </td> <td>0          </td>
        </tr>
    </tbody>
        <tr>
            <td>1   </td> <td>0   </td> <td>0.125      </td>
        </tr>
    </tbody>
        <tr>
            <td>1   </td> <td>1   </td> <td>0.25       </td>
        </tr>
    </tbody>
        <tr>
            <td>1   </td> <td>2   </td> <td>0.125      </td>
        </tr>
    </tbody>
        <tr>
            <td>2   </td> <td>0   </td> <td>0          </td>
        </tr>
    </tbody>
        <tr>
            <td>2   </td> <td>1   </td> <td>0.125      </td>
        </tr>
    </tbody>
        <tr>
            <td>2   </td> <td>2   </td> <td>0.125      </td>
        </tr>
    </tbody>
</table>
</div>



This table displays the joint distribution. To check that this is indeed a distribution, we can add up all the probabilities. The sum is 1, as it should be for a distribution.


{:.input_area}
```python
joint_table.column(2).sum()
```




{:.output_data_text}
```
1.0
```



### Joint Distribution Table ###
Though the table above does display the joint distribution, it is more conventional and also more illuminating to visualize the same information in a different way.

The method `to_joint` converts the table above into a "joint distribution object" that is displayed as a conventional *joint distribution table* for $X$ and $Y$.


{:.input_area}
```python
joint_dist = joint_table.to_joint()
joint_dist
```




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>X=0</th>
      <th>X=1</th>
      <th>X=2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Y=2</th>
      <td>0.000</td>
      <td>0.125</td>
      <td>0.125</td>
    </tr>
    <tr>
      <th>Y=1</th>
      <td>0.125</td>
      <td>0.250</td>
      <td>0.125</td>
    </tr>
    <tr>
      <th>Y=0</th>
      <td>0.125</td>
      <td>0.125</td>
      <td>0.000</td>
    </tr>
  </tbody>
</table>
</div>
</div>



Each cell corresponds to a pair $(x, y)$, where $x$ is a value of $X$ and $y$ a value of $Y$. In the cell you see $P(X = x, Y = y)$, the probability of the pair $(x, y)$. 

Joint distribution tables are analogous to the contingency tables you saw in Data 8 when you were analyzing the relation between two categorical variables. In contingency tables, each cell contains the number of individuals in one particular pair of categories. In joint distribution tables, such as the one above, each cell contains the probability of one particular pair of values.

### Finding Probabilities ###
The table contains complete information about the relation between $X$ and $Y$. To find the probabiilty of any event determined by $X$ and $Y$, simply identify the cells that make the event happen, and add up their chances. This is an application of the fundamental method of finding probabilities by partitioning an event.

For example,

\begin{align*}
P(X > Y ) &= P(X = 1, Y = 0) + P(X = 2, Y = 0) + P(X = 2 , Y = 1) \\
&= 0.125 + 0 + 0.125 \\
&= 0.25
\end{align*}

And

\begin{align*}
P(X = Y ) &= P(X = 0, Y = 0) + P(X = 1, Y = 1) + P(X = 2 , Y = 2) \\
&= 0.125 + 0.25 + 0.125 \\
&= 0.5
\end{align*}