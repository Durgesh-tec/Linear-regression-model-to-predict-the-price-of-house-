# Linear-regression
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Linear Regression Machine Learning Project for House Price Prediction"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Import Libraries"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [],
   "source": [
    "import pandas as pd\n",
    "import numpy as np\n",
    "import seaborn as sns\n",
    "import matplotlib.pyplot as plt\n",
    "\n",
    "%matplotlib inline"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Importing Data and Checking out."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
   "metadata": {},
   "outputs": [],
   "source": [
    "HouseDF = pd.read_csv('USA_Housing.csv')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 50,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Avg. Area Income</th>\n",
       "      <th>Avg. Area House Age</th>\n",
       "      <th>Avg. Area Number of Rooms</th>\n",
       "      <th>Avg. Area Number of Bedrooms</th>\n",
       "      <th>Area Population</th>\n",
       "      <th>Price</th>\n",
       "      <th>Address</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>79545.458574</td>\n",
       "      <td>5.682861</td>\n",
       "      <td>7.009188</td>\n",
       "      <td>4.09</td>\n",
       "      <td>23086.800503</td>\n",
       "      <td>1.059034e+06</td>\n",
       "      <td>208 Michael Ferry Apt. 674\\nLaurabury, NE 3701...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>79248.642455</td>\n",
       "      <td>6.002900</td>\n",
       "      <td>6.730821</td>\n",
       "      <td>3.09</td>\n",
       "      <td>40173.072174</td>\n",
       "      <td>1.505891e+06</td>\n",
       "      <td>188 Johnson Views Suite 079\\nLake Kathleen, CA...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>61287.067179</td>\n",
       "      <td>5.865890</td>\n",
       "      <td>8.512727</td>\n",
       "      <td>5.13</td>\n",
       "      <td>36882.159400</td>\n",
       "      <td>1.058988e+06</td>\n",
       "      <td>9127 Elizabeth Stravenue\\nDanieltown, WI 06482...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>63345.240046</td>\n",
       "      <td>7.188236</td>\n",
       "      <td>5.586729</td>\n",
       "      <td>3.26</td>\n",
       "      <td>34310.242831</td>\n",
       "      <td>1.260617e+06</td>\n",
       "      <td>USS Barnett\\nFPO AP 44820</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>59982.197226</td>\n",
       "      <td>5.040555</td>\n",
       "      <td>7.839388</td>\n",
       "      <td>4.23</td>\n",
       "      <td>26354.109472</td>\n",
       "      <td>6.309435e+05</td>\n",
       "      <td>USNS Raymond\\nFPO AE 09386</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   Avg. Area Income  Avg. Area House Age  Avg. Area Number of Rooms  \\\n",
       "0      79545.458574             5.682861                   7.009188   \n",
       "1      79248.642455             6.002900                   6.730821   \n",
       "2      61287.067179             5.865890                   8.512727   \n",
       "3      63345.240046             7.188236                   5.586729   \n",
       "4      59982.197226             5.040555                   7.839388   \n",
       "\n",
       "   Avg. Area Number of Bedrooms  Area Population         Price  \\\n",
       "0                          4.09     23086.800503  1.059034e+06   \n",
       "1                          3.09     40173.072174  1.505891e+06   \n",
       "2                          5.13     36882.159400  1.058988e+06   \n",
       "3                          3.26     34310.242831  1.260617e+06   \n",
       "4                          4.23     26354.109472  6.309435e+05   \n",
       "\n",
       "                                             Address  \n",
       "0  208 Michael Ferry Apt. 674\\nLaurabury, NE 3701...  \n",
       "1  188 Johnson Views Suite 079\\nLake Kathleen, CA...  \n",
       "2  9127 Elizabeth Stravenue\\nDanieltown, WI 06482...  \n",
       "3                          USS Barnett\\nFPO AP 44820  \n",
       "4                         USNS Raymond\\nFPO AE 09386  "
      ]
     },
     "execution_count": 50,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "HouseDF.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 5000 entries, 0 to 4999\n",
      "Data columns (total 7 columns):\n",
      "Avg. Area Income                5000 non-null float64\n",
      "Avg. Area House Age             5000 non-null float64\n",
      "Avg. Area Number of Rooms       5000 non-null float64\n",
      "Avg. Area Number of Bedrooms    5000 non-null float64\n",
      "Area Population                 5000 non-null float64\n",
      "Price                           5000 non-null float64\n",
      "Address                         5000 non-null object\n",
      "dtypes: float64(6), object(1)\n",
      "memory usage: 273.6+ KB\n"
     ]
    }
   ],
   "source": [
    "HouseDF.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Avg. Area Income</th>\n",
       "      <th>Avg. Area House Age</th>\n",
       "      <th>Avg. Area Number of Rooms</th>\n",
       "      <th>Avg. Area Number of Bedrooms</th>\n",
       "      <th>Area Population</th>\n",
       "      <th>Price</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>5000.000000</td>\n",
       "      <td>5000.000000</td>\n",
       "      <td>5000.000000</td>\n",
       "      <td>5000.000000</td>\n",
       "      <td>5000.000000</td>\n",
       "      <td>5.000000e+03</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>68583.108984</td>\n",
       "      <td>5.977222</td>\n",
       "      <td>6.987792</td>\n",
       "      <td>3.981330</td>\n",
       "      <td>36163.516039</td>\n",
       "      <td>1.232073e+06</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>10657.991214</td>\n",
       "      <td>0.991456</td>\n",
       "      <td>1.005833</td>\n",
       "      <td>1.234137</td>\n",
       "      <td>9925.650114</td>\n",
       "      <td>3.531176e+05</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>17796.631190</td>\n",
       "      <td>2.644304</td>\n",
       "      <td>3.236194</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>172.610686</td>\n",
       "      <td>1.593866e+04</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>61480.562388</td>\n",
       "      <td>5.322283</td>\n",
       "      <td>6.299250</td>\n",
       "      <td>3.140000</td>\n",
       "      <td>29403.928702</td>\n",
       "      <td>9.975771e+05</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>68804.286404</td>\n",
       "      <td>5.970429</td>\n",
       "      <td>7.002902</td>\n",
       "      <td>4.050000</td>\n",
       "      <td>36199.406689</td>\n",
       "      <td>1.232669e+06</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>75783.338666</td>\n",
       "      <td>6.650808</td>\n",
       "      <td>7.665871</td>\n",
       "      <td>4.490000</td>\n",
       "      <td>42861.290769</td>\n",
       "      <td>1.471210e+06</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>107701.748378</td>\n",
       "      <td>9.519088</td>\n",
       "      <td>10.759588</td>\n",
       "      <td>6.500000</td>\n",
       "      <td>69621.713378</td>\n",
       "      <td>2.469066e+06</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       Avg. Area Income  Avg. Area House Age  Avg. Area Number of Rooms  \\\n",
       "count       5000.000000          5000.000000                5000.000000   \n",
       "mean       68583.108984             5.977222                   6.987792   \n",
       "std        10657.991214             0.991456                   1.005833   \n",
       "min        17796.631190             2.644304                   3.236194   \n",
       "25%        61480.562388             5.322283                   6.299250   \n",
       "50%        68804.286404             5.970429                   7.002902   \n",
       "75%        75783.338666             6.650808                   7.665871   \n",
       "max       107701.748378             9.519088                  10.759588   \n",
       "\n",
       "       Avg. Area Number of Bedrooms  Area Population         Price  \n",
       "count                   5000.000000      5000.000000  5.000000e+03  \n",
       "mean                       3.981330     36163.516039  1.232073e+06  \n",
       "std                        1.234137      9925.650114  3.531176e+05  \n",
       "min                        2.000000       172.610686  1.593866e+04  \n",
       "25%                        3.140000     29403.928702  9.975771e+05  \n",
       "50%                        4.050000     36199.406689  1.232669e+06  \n",
       "75%                        4.490000     42861.290769  1.471210e+06  \n",
       "max                        6.500000     69621.713378  2.469066e+06  "
      ]
     },
     "execution_count": 52,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "HouseDF.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['Avg. Area Income', 'Avg. Area House Age', 'Avg. Area Number of Rooms',\n",
       "       'Avg. Area Number of Bedrooms', 'Area Population', 'Price', 'Address'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 53,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "HouseDF.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Exploratory Data Analysis for House Price Prediction"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<seaborn.axisgrid.PairGrid at 0x67b60e8d08>"
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": 
      "text/plain": [
       "<Figure size 1080x1080 with 42 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.pairplot(HouseDF)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x67b80b5648>"
      ]
     },
     "execution_count": 55,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": 
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.distplot(HouseDF['Price'])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.axes._subplots.AxesSubplot at 0x67b868c948>"
      ]
     },
     "execution_count": 56,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": /
      "text/plain": [
       "<Figure size 432x288 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.heatmap(HouseDF.corr(), annot=True)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Training a Linear Regression Model\n",
    "\n",
    "### X and y List"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [],
   "source": [
    "X = HouseDF[['Avg. Area Income', 'Avg. Area House Age', 'Avg. Area Number of Rooms',\n",
    "               'Avg. Area Number of Bedrooms', 'Area Population']]\n",
    "\n",
    "y = HouseDF['Price']"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Split Data into Train, Test"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.model_selection import train_test_split"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, random_state=101)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Creating and Training the LinearRegression Model"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 60,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.linear_model import LinearRegression"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 61,
   "metadata": {},
   "outputs": [],
   "source": [
    "lm = LinearRegression()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 62,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)"
      ]
     },
     "execution_count": 62,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "lm.fit(X_train,y_train)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## LinearRegression Model Evaluation"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 63,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "-2640159.796851911\n"
     ]
    }
   ],
   "source": [
    "print(lm.intercept_)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Coefficient</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>Avg. Area Income</th>\n",
       "      <td>21.528276</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Avg. Area House Age</th>\n",
       "      <td>164883.282027</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Avg. Area Number of Rooms</th>\n",
       "      <td>122368.678027</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Avg. Area Number of Bedrooms</th>\n",
       "      <td>2233.801864</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>Area Population</th>\n",
       "      <td>15.150420</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                                Coefficient\n",
       "Avg. Area Income                  21.528276\n",
       "Avg. Area House Age           164883.282027\n",
       "Avg. Area Number of Rooms     122368.678027\n",
       "Avg. Area Number of Bedrooms    2233.801864\n",
       "Area Population                   15.150420"
      ]
     },
     "execution_count": 64,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "coeff_df = pd.DataFrame(lm.coef_,X.columns,columns=['Coefficient'])\n",
    "coeff_df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Predictions from our Linear Regression Model"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 65,
   "metadata": {},
   "outputs": [],
   "source": [
    "predictions = lm.predict(X_test)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 66,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<matplotlib.collections.PathCollection at 0x67b87ccc88>"
      ]
     },
     "execution_count": 66,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAAAZAAAAD4CAYAAADCb7BPAAAABHNCSVQICA
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.scatter(y_test,predictions)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In the above scatter plot, we see data is in line shape, which means our model has done good predictions."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 67,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": 
      "text/plain": [
       "<Figure size 432x288 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sns.distplot((y_test-predictions),bins=50);"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "In the above histogram plot, we see data is in bell shape (Normally Distributed), which means our model has done good predictions."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Regression Evaluation Metrics"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 68,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn import metrics"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 69,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "MAE: 82288.22251914957\n",
      "MSE: 10460958907.209501\n",
      "RMSE: 102278.82922291153\n"
     ]
    }
   ],
   "source": [
    "print('MAE:', metrics.mean_absolute_error(y_test, predictions))\n",
    "print('MSE:', metrics.mean_squared_error(y_test, predictions))\n",
    "print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, predictions)))"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.4"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
} 
