# 31 - Decision Trees

## Kya hai Decision Tree?
Decision Tree ek supervised ML algorithm hai jo data ko step-by-step sawal-jawab
(if-else jaisa) ke through split karta hai jab tak final decision (prediction) tak
na pahunch jaye. Bilkul jaise doctor patient se sawal poochta hai symptoms ke
hisaab se, phir final diagnosis deta hai.

## Entropy
Entropy ek group mein "confusion" ya "impurity" ko measure karti hai.
- Entropy = 0  -> group bilkul pure hai (sab same class)
- Entropy = 1  -> group mein sabse zyada confusion hai (50-50 mix)

Formula: Entropy = - sum( p_i * log2(p_i) )
jahan p_i har class ka proportion hai group mein.

## Gini Impurity
Gini bhi impurity measure karti hai, Entropy jaisa hi kaam, bas formula alag
aur calculate karne mein fast hai. Sklearn default isi ko use karta hai.

Formula: Gini = 1 - sum( p_i^2 )

## Information Gain
Information Gain batata hai ke ek sawal (split) poochne se confusion (entropy)
kitna kam hua.

Information Gain = Entropy(parent) - Weighted Entropy(children)

Jis feature se sabse zyada Information Gain milta hai, tree usi ko pehle
sawal (root node) banata hai.

## Types of Decision Trees
- **Classification Tree (Categorical)**: target Yes/No jaisi categories mein
  hota hai. Hamare dataset mein `Attrition` (Yes/No) is type ka example hai.
- **Regression Tree (Numerical)**: target ek number hota hai, jaise Salary.
- **Mixed Tree**: features numerical aur categorical dono ho sakte hain,
  target ek type ka - Decision Tree dono handle kar sakta hai.

## Important Hyperparameters
| Hyperparameter | Kya karta hai |
|---|---|
| criterion | Split ka tareeqa: "gini" ya "entropy" |
| max_depth | Tree kitni depth (levels) tak jaye |
| min_samples_split | Node split ke liye minimum samples |
| min_samples_leaf | Har leaf mein minimum samples |
| max_features | Har split pe kitne features consider hon |
| max_leaf_nodes | Total leaves ki max limit |
| min_impurity_decrease | Split tabhi ho jab impurity itni kam ho |
| splitter | "best" ya "random" split strategy |
| random_state | Reproducibility ke liye fixed seed |
| class_weight | Imbalanced classes ko balance karne ke liye |

## Overfitting vs Underfitting
- **Overfitting**: Tree bohot deep, training data ratta laga leta hai,
  naye data pe perform nahi karta.
- **Underfitting**: Tree bohot shallow, kuch bhi acche se seekha hi nahi.

## Dataset Used
`employee_data_cleaned.csv` - 500 rows, 20 columns.
Target column: `Attrition` (Yes/No) - Classification problem.
Imbalanced target: No=405, Yes=95 -> is liye class_weight='balanced' use hoga.

