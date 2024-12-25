# bike-sharing
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    " # <span style = 'color : Green' >Bike Sharing Assignment by  Syed Sha Khalid "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## **Problem Statement**<br>\n",
    "\n",
    "A bike-sharing system is a service in which bikes are made available for shared use to individuals on a short term basis for a price or free. Many bike share systems allow people to borrow a bike from a \"dock\" which is usually computer-controlled wherein the user enters the payment information, and the system unlocks it. This bike can then be returned to another dock belonging to the same system.\n",
    "\n",
    "\n",
    "A US bike-sharing provider **BoomBikes** has recently suffered considerable dips in their revenues due to the ongoing Corona pandemic. The company is finding it very difficult to sustain in the current market scenario. So, it has decided to come up with a mindful business plan to be able to accelerate its revenue as soon as the ongoing lockdown comes to an end, and the economy restores to a healthy state. \n",
    "\n",
    "\n",
    "In such an attempt, BoomBikes aspires to understand the demand for shared bikes among the people after this ongoing quarantine situation ends across the nation due to Covid-19. They have planned this to prepare themselves to cater to the people's needs once the situation gets better all around and stand out from other service providers and make huge profits.\n",
    "\n",
    "\n",
    "They have contracted a consulting company to understand the factors on which the demand for these shared bikes depends. Specifically, they want to understand the factors affecting the demand for these shared bikes in the American market. The company wants to know:\n",
    "\n",
    "* Which variables are significant in predicting the demand for shared bikes.\n",
    "* How well those variables describe the bike demands <br>\n",
    "\n",
    "Based on various meteorological surveys and people's styles, the service provider firm has gathered a large dataset on daily bike demands across the American market based on some factors. \n",
    "\n",
    "\n",
    "**Business Goal**:<br>\n",
    "You are required to model the demand for shared bikes with the available independent variables. It will be used by the management to understand how exactly the demands vary with different features. They can accordingly manipulate the business strategy to meet the demand levels and meet the customer's expectations. Further, the model will be a good way for management to understand the demand dynamics of a new market. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Importing required libraries"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Supress Warnings\n",
    "\n",
    "import warnings\n",
    "warnings.filterwarnings('ignore')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "import numpy as np\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import seaborn as sns\n",
    "%matplotlib inline\n",
    "import sklearn\n",
    "from sklearn.feature_selection import RFE\n",
    "from sklearn.linear_model import LinearRegression\n",
    "from sklearn.model_selection import train_test_split\n",
    "from sklearn.preprocessing import MinMaxScaler\n",
    "import statsmodels.api as sm\n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "from sklearn.metrics import r2_score"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > Reading and Understanding the Data\n",
    "## Read Bike Sharing Dataset file that is \"day.csv\" as bike"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "bike = pd.read_csv(\"day.csv\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style = 'color : Red' > Exploratory Data Analysis"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
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
       "      <th>instant</th>\n",
       "      <th>dteday</th>\n",
       "      <th>season</th>\n",
       "      <th>yr</th>\n",
       "      <th>mnth</th>\n",
       "      <th>holiday</th>\n",
       "      <th>weekday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>weathersit</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>casual</th>\n",
       "      <th>registered</th>\n",
       "      <th>cnt</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1</td>\n",
       "      <td>01-01-2018</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>2</td>\n",
       "      <td>14.110847</td>\n",
       "      <td>18.18125</td>\n",
       "      <td>80.5833</td>\n",
       "      <td>10.749882</td>\n",
       "      <td>331</td>\n",
       "      <td>654</td>\n",
       "      <td>985</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2</td>\n",
       "      <td>02-01-2018</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>2</td>\n",
       "      <td>1</td>\n",
       "      <td>2</td>\n",
       "      <td>14.902598</td>\n",
       "      <td>17.68695</td>\n",
       "      <td>69.6087</td>\n",
       "      <td>16.652113</td>\n",
       "      <td>131</td>\n",
       "      <td>670</td>\n",
       "      <td>801</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>3</td>\n",
       "      <td>03-01-2018</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>3</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>8.050924</td>\n",
       "      <td>9.47025</td>\n",
       "      <td>43.7273</td>\n",
       "      <td>16.636703</td>\n",
       "      <td>120</td>\n",
       "      <td>1229</td>\n",
       "      <td>1349</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>4</td>\n",
       "      <td>04-01-2018</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>4</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>8.200000</td>\n",
       "      <td>10.60610</td>\n",
       "      <td>59.0435</td>\n",
       "      <td>10.739832</td>\n",
       "      <td>108</td>\n",
       "      <td>1454</td>\n",
       "      <td>1562</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>5</td>\n",
       "      <td>05-01-2018</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>5</td>\n",
       "      <td>1</td>\n",
       "      <td>1</td>\n",
       "      <td>9.305237</td>\n",
       "      <td>11.46350</td>\n",
       "      <td>43.6957</td>\n",
       "      <td>12.522300</td>\n",
       "      <td>82</td>\n",
       "      <td>1518</td>\n",
       "      <td>1600</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   instant      dteday  season  yr  mnth  holiday  weekday  workingday  \\\n",
       "0        1  01-01-2018       1   0     1        0        1           1   \n",
       "1        2  02-01-2018       1   0     1        0        2           1   \n",
       "2        3  03-01-2018       1   0     1        0        3           1   \n",
       "3        4  04-01-2018       1   0     1        0        4           1   \n",
       "4        5  05-01-2018       1   0     1        0        5           1   \n",
       "\n",
       "   weathersit       temp     atemp      hum  windspeed  casual  registered  \\\n",
       "0           2  14.110847  18.18125  80.5833  10.749882     331         654   \n",
       "1           2  14.902598  17.68695  69.6087  16.652113     131         670   \n",
       "2           1   8.050924   9.47025  43.7273  16.636703     120        1229   \n",
       "3           1   8.200000  10.60610  59.0435  10.739832     108        1454   \n",
       "4           1   9.305237  11.46350  43.6957  12.522300      82        1518   \n",
       "\n",
       "    cnt  \n",
       "0   985  \n",
       "1   801  \n",
       "2  1349  \n",
       "3  1562  \n",
       "4  1600  "
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check the head of the dataset\n",
    "bike.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 730 entries, 0 to 729\n",
      "Data columns (total 16 columns):\n",
      " #   Column      Non-Null Count  Dtype  \n",
      "---  ------      --------------  -----  \n",
      " 0   instant     730 non-null    int64  \n",
      " 1   dteday      730 non-null    object \n",
      " 2   season      730 non-null    int64  \n",
      " 3   yr          730 non-null    int64  \n",
      " 4   mnth        730 non-null    int64  \n",
      " 5   holiday     730 non-null    int64  \n",
      " 6   weekday     730 non-null    int64  \n",
      " 7   workingday  730 non-null    int64  \n",
      " 8   weathersit  730 non-null    int64  \n",
      " 9   temp        730 non-null    float64\n",
      " 10  atemp       730 non-null    float64\n",
      " 11  hum         730 non-null    float64\n",
      " 12  windspeed   730 non-null    float64\n",
      " 13  casual      730 non-null    int64  \n",
      " 14  registered  730 non-null    int64  \n",
      " 15  cnt         730 non-null    int64  \n",
      "dtypes: float64(4), int64(11), object(1)\n",
      "memory usage: 91.4+ KB\n"
     ]
    }
   ],
   "source": [
    "# Check the descriptive information\n",
    "bike.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
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
       "      <th>instant</th>\n",
       "      <th>season</th>\n",
       "      <th>yr</th>\n",
       "      <th>mnth</th>\n",
       "      <th>holiday</th>\n",
       "      <th>weekday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>weathersit</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>casual</th>\n",
       "      <th>registered</th>\n",
       "      <th>cnt</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "      <td>730.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>365.500000</td>\n",
       "      <td>2.498630</td>\n",
       "      <td>0.500000</td>\n",
       "      <td>6.526027</td>\n",
       "      <td>0.028767</td>\n",
       "      <td>2.995890</td>\n",
       "      <td>0.690411</td>\n",
       "      <td>1.394521</td>\n",
       "      <td>20.319259</td>\n",
       "      <td>23.726322</td>\n",
       "      <td>62.765175</td>\n",
       "      <td>12.763620</td>\n",
       "      <td>849.249315</td>\n",
       "      <td>3658.757534</td>\n",
       "      <td>4508.006849</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>210.877136</td>\n",
       "      <td>1.110184</td>\n",
       "      <td>0.500343</td>\n",
       "      <td>3.450215</td>\n",
       "      <td>0.167266</td>\n",
       "      <td>2.000339</td>\n",
       "      <td>0.462641</td>\n",
       "      <td>0.544807</td>\n",
       "      <td>7.506729</td>\n",
       "      <td>8.150308</td>\n",
       "      <td>14.237589</td>\n",
       "      <td>5.195841</td>\n",
       "      <td>686.479875</td>\n",
       "      <td>1559.758728</td>\n",
       "      <td>1936.011647</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>2.424346</td>\n",
       "      <td>3.953480</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.500244</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>20.000000</td>\n",
       "      <td>22.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>183.250000</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>13.811885</td>\n",
       "      <td>16.889713</td>\n",
       "      <td>52.000000</td>\n",
       "      <td>9.041650</td>\n",
       "      <td>316.250000</td>\n",
       "      <td>2502.250000</td>\n",
       "      <td>3169.750000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>365.500000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>0.500000</td>\n",
       "      <td>7.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>20.465826</td>\n",
       "      <td>24.368225</td>\n",
       "      <td>62.625000</td>\n",
       "      <td>12.125325</td>\n",
       "      <td>717.000000</td>\n",
       "      <td>3664.500000</td>\n",
       "      <td>4548.500000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>547.750000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>10.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>5.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>2.000000</td>\n",
       "      <td>26.880615</td>\n",
       "      <td>30.445775</td>\n",
       "      <td>72.989575</td>\n",
       "      <td>15.625589</td>\n",
       "      <td>1096.500000</td>\n",
       "      <td>4783.250000</td>\n",
       "      <td>5966.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>730.000000</td>\n",
       "      <td>4.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>12.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>6.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>3.000000</td>\n",
       "      <td>35.328347</td>\n",
       "      <td>42.044800</td>\n",
       "      <td>97.250000</td>\n",
       "      <td>34.000021</td>\n",
       "      <td>3410.000000</td>\n",
       "      <td>6946.000000</td>\n",
       "      <td>8714.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "          instant      season          yr        mnth     holiday     weekday  \\\n",
       "count  730.000000  730.000000  730.000000  730.000000  730.000000  730.000000   \n",
       "mean   365.500000    2.498630    0.500000    6.526027    0.028767    2.995890   \n",
       "std    210.877136    1.110184    0.500343    3.450215    0.167266    2.000339   \n",
       "min      1.000000    1.000000    0.000000    1.000000    0.000000    0.000000   \n",
       "25%    183.250000    2.000000    0.000000    4.000000    0.000000    1.000000   \n",
       "50%    365.500000    3.000000    0.500000    7.000000    0.000000    3.000000   \n",
       "75%    547.750000    3.000000    1.000000   10.000000    0.000000    5.000000   \n",
       "max    730.000000    4.000000    1.000000   12.000000    1.000000    6.000000   \n",
       "\n",
       "       workingday  weathersit        temp       atemp         hum   windspeed  \\\n",
       "count  730.000000  730.000000  730.000000  730.000000  730.000000  730.000000   \n",
       "mean     0.690411    1.394521   20.319259   23.726322   62.765175   12.763620   \n",
       "std      0.462641    0.544807    7.506729    8.150308   14.237589    5.195841   \n",
       "min      0.000000    1.000000    2.424346    3.953480    0.000000    1.500244   \n",
       "25%      0.000000    1.000000   13.811885   16.889713   52.000000    9.041650   \n",
       "50%      1.000000    1.000000   20.465826   24.368225   62.625000   12.125325   \n",
       "75%      1.000000    2.000000   26.880615   30.445775   72.989575   15.625589   \n",
       "max      1.000000    3.000000   35.328347   42.044800   97.250000   34.000021   \n",
       "\n",
       "            casual   registered          cnt  \n",
       "count   730.000000   730.000000   730.000000  \n",
       "mean    849.249315  3658.757534  4508.006849  \n",
       "std     686.479875  1559.758728  1936.011647  \n",
       "min       2.000000    20.000000    22.000000  \n",
       "25%     316.250000  2502.250000  3169.750000  \n",
       "50%     717.000000  3664.500000  4548.500000  \n",
       "75%    1096.500000  4783.250000  5966.000000  \n",
       "max    3410.000000  6946.000000  8714.000000  "
      ]
     },
     "execution_count": 6,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "(730, 16)\n"
     ]
    }
   ],
   "source": [
    "# Check the shape of data frame \n",
    "\n",
    "print(bike.shape)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '> Finding</span> :\n",
    "\n",
    "Dataset has 730 rows and 16 columns.\n",
    "\n",
    "Except one column that is \"dteday\", all other are either float or integer type.\n",
    "\n",
    "One column is date type.\n",
    "\n",
    "Looking at the data, there seems to be some fields that are categorical in nature, but in integer/float type.\n",
    "\n",
    "We will analyse and finalize whether to convert them to categorical or treat as integer.\n",
    "\n",
    "# DATA QUALITY CHECK\n",
    "<span style='background: lightGreen '> Check for NULL/MISSING values</span>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "cnt           0.0\n",
       "registered    0.0\n",
       "casual        0.0\n",
       "windspeed     0.0\n",
       "hum           0.0\n",
       "atemp         0.0\n",
       "temp          0.0\n",
       "weathersit    0.0\n",
       "workingday    0.0\n",
       "weekday       0.0\n",
       "holiday       0.0\n",
       "mnth          0.0\n",
       "yr            0.0\n",
       "season        0.0\n",
       "dteday        0.0\n",
       "instant       0.0\n",
       "dtype: float64"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# percentage of missing values in each column\n",
    "#bike.isnull().sum()*100/bike.shape[0]\n",
    "#or\n",
    "round(100*(bike.isnull().sum()/len(bike)), 2).sort_values(ascending=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "729    0.0\n",
       "250    0.0\n",
       "248    0.0\n",
       "247    0.0\n",
       "246    0.0\n",
       "      ... \n",
       "484    0.0\n",
       "483    0.0\n",
       "482    0.0\n",
       "481    0.0\n",
       "0      0.0\n",
       "Length: 730, dtype: float64"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# row-wise null count percentage\n",
    "round(100*(bike.isnull().sum(axis=1)/len(bike)),2).sort_values(ascending=False)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### Conclusion: There are no missing / Null values either in columns or rows"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '> Duplicate Check</span>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [],
   "source": [
    "bike_dupl = bike.copy()\n",
    "\n",
    "# Checking for duplicates and dropping the entire duplicate row if any\n",
    "bike_dupl.drop_duplicates(subset=None, inplace=True)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(730, 16)"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike_dupl.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(730, 16)"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The shape after running the drop duplicate command is same as the original dataframe.\n",
    "### Hence we can conclude that there were zero duplicate values in the dataset."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Data Cleaning: \n",
    "    \n",
    " <span style='background: lightGreen '>Checking value_counts() for entire dataframe</span>.\n",
    "\n",
    "This will help to identify any Unknow/Junk values present in the dataset"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "instant  dteday      season  yr  mnth  holiday  weekday  workingday  weathersit  temp      atemp     hum    windspeed  casual  registered  cnt \n",
       "730      31-12-2019  1       1   12    0        2        1           2           8.849153  11.17435  57.75  10.374682  439     2290        2729    1\n",
       "dtype: int64"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike.value_counts(ascending=False).head(1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "There seems to be no Junk/Unknown values in the entire dataset."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    " <span style='background: lightGreen '> Removing redundant & unwanted columns</span>\n",
    "\n",
    "Based on the high level look at the data and the data dictionary, the following variables can be removed from further analysis:\n",
    "\n",
    "1) instant : Its only an index value\n",
    "\n",
    "2) dteday : This has the date, Since we already have seperate columns for 'year' & 'month',hence, we could live without this column.\n",
    "\n",
    "3) casual & registered : Both these columns contains the count of bike booked by different categories of customers. Since our objective is to find the total count of bikes and not by specific category, we will ignore these two columns. More over, we have created a new variable to have the ratio of these customer types.\n",
    "\n",
    "4) We will save the new dataframe as bike_new, so that the original dataset is preserved for any future analysis/validation"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['instant', 'dteday', 'season', 'yr', 'mnth', 'holiday', 'weekday',\n",
       "       'workingday', 'weathersit', 'temp', 'atemp', 'hum', 'windspeed',\n",
       "       'casual', 'registered', 'cnt'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "New daraframe without 'instant', 'dteday', 'casual' & 'registered' columns as bike_new"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [],
   "source": [
    "bike_new=bike[['season', 'yr', 'mnth', 'holiday', 'weekday','workingday', 'weathersit', 'temp',\n",
    "               'atemp', 'hum', 'windspeed','cnt']]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 16,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(730, 12)"
      ]
     },
     "execution_count": 16,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike_new.shape"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '> Conver **int64** to **Catagorical** Variables </span>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 730 entries, 0 to 729\n",
      "Data columns (total 12 columns):\n",
      " #   Column      Non-Null Count  Dtype  \n",
      "---  ------      --------------  -----  \n",
      " 0   season      730 non-null    int64  \n",
      " 1   yr          730 non-null    int64  \n",
      " 2   mnth        730 non-null    int64  \n",
      " 3   holiday     730 non-null    int64  \n",
      " 4   weekday     730 non-null    int64  \n",
      " 5   workingday  730 non-null    int64  \n",
      " 6   weathersit  730 non-null    int64  \n",
      " 7   temp        730 non-null    float64\n",
      " 8   atemp       730 non-null    float64\n",
      " 9   hum         730 non-null    float64\n",
      " 10  windspeed   730 non-null    float64\n",
      " 11  cnt         730 non-null    int64  \n",
      "dtypes: float64(4), int64(8)\n",
      "memory usage: 68.6 KB\n"
     ]
    }
   ],
   "source": [
    "# Check the datatypes before convertion\n",
    "bike_new.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* season','mnth','weekday','weathersit' are catagorical variables not int64 hence convreting them to catagorical "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Fall      188\n",
       "Summer    184\n",
       "Spring    180\n",
       "Winter    178\n",
       "Name: season, dtype: int64"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Converting 'Season' to a categorical variable\n",
    "bike_new['season'].replace([1, 2, 3, 4], ['Spring', 'Summer', 'Fall', 'Winter'], inplace = True)\n",
    "bike_new['season'].value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 19,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep',\n",
       "       'Oct', 'Nov', 'Dec'], dtype=object)"
      ]
     },
     "execution_count": 19,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Converting 'mnth' to categorical variable \n",
    "\n",
    "import calendar\n",
    "\n",
    "bike_new['mnth'] = bike_new['mnth'].apply(lambda x: calendar.month_abbr[x])\n",
    "bike_new['mnth'].unique()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Converting 'weekday' to objectin preparation for making dummy variable\n",
    "\n",
    "bike_new['weekday'] = bike_new['weekday'].astype('object')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Clear              463\n",
       "Mist_Cloudy        246\n",
       "Light_Snow_Rain     21\n",
       "Name: weathersit, dtype: int64"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Converting 'weathersit' to a categorical variable\n",
    "\n",
    "bike_new['weathersit']=bike_new['weathersit'].replace([1, 2, 3], ['Clear', 'Mist_Cloudy', 'Light_Snow_Rain'])\n",
    "bike_new['weathersit'].value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 730 entries, 0 to 729\n",
      "Data columns (total 12 columns):\n",
      " #   Column      Non-Null Count  Dtype  \n",
      "---  ------      --------------  -----  \n",
      " 0   season      730 non-null    object \n",
      " 1   yr          730 non-null    int64  \n",
      " 2   mnth        730 non-null    object \n",
      " 3   holiday     730 non-null    int64  \n",
      " 4   weekday     730 non-null    object \n",
      " 5   workingday  730 non-null    int64  \n",
      " 6   weathersit  730 non-null    object \n",
      " 7   temp        730 non-null    float64\n",
      " 8   atemp       730 non-null    float64\n",
      " 9   hum         730 non-null    float64\n",
      " 10  windspeed   730 non-null    float64\n",
      " 11  cnt         730 non-null    int64  \n",
      "dtypes: float64(4), int64(4), object(4)\n",
      "memory usage: 68.6+ KB\n"
     ]
    }
   ],
   "source": [
    "bike_new.info()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##  <span style = 'color : Green' > Univariate Analysis"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '>Visualizing Binary Columns (Numerical Variables)</span>  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1152x1080 with 3 Axes>"
      ]
     },
     "metadata": {},
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = [16,15])\n",
    "plt.subplot(131)\n",
    "bike_new['yr'].value_counts(normalize = True).plot.pie(explode=(0.05, 0), autopct = \"%1.0f%%\", startangle=10)\n",
    "plt.subplot(132) \n",
    "bike_new['holiday'].value_counts(normalize = True).plot.pie(explode=(0.4, 0), autopct = \"%1.0f%%\", startangle=110)\n",
    "plt.subplot(133)\n",
    "bike_new['workingday'].value_counts(normalize = True).plot.pie(explode=(0.05, 0),autopct = \"%1.0f%%\", startangle=10)\n",
    "plt.title('yr, holiday and Workingday',fontsize=30)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* By observing the 3 pi charts we can came to conclusion that \n",
    "    - 'Yr' is expected to be 50%-50% daily records of bike usage. \n",
    "    - Significantly less number of holidays(1) as compared to non-holidays(0) hence bike usage is more in 0. \n",
    "    - The same case applies to 'workingday' due to higher number of days vs non-working days."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '>Visualizing Binary Columns (Categorical Variables)  </span>  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1440x288 with 4 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = [20,4])\n",
    "plt.subplot(141)\n",
    "sns.countplot(data = bike_new, x = 'weathersit')\n",
    "plt.subplot(142)\n",
    "sns.countplot(data = bike_new, x = 'season')\n",
    "plt.subplot(143)\n",
    "sns.countplot(data = bike_new, x = 'weekday')\n",
    "plt.subplot(144)\n",
    "plt.xticks(rotation = 45)\n",
    "sns.countplot(data = bike_new, x = 'mnth')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* By observing the plots we can came to conclusion that\n",
    "    - When 'weathersit' is Clear, Few clouds, Partly cloudy, Partly cloudy the bikes are usage are more. \n",
    "    - The rest of the variables are shows very close values."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##  <span style = 'color : Green' > Bivariate Analysis"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '>Visualizing Numerical Variables vs 'cnt' </span>  "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1152x288 with 3 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = [16,4])\n",
    "plt.subplot(131)\n",
    "sns.barplot('yr', 'cnt', data = bike_new )\n",
    "plt.subplot(132)\n",
    "sns.barplot('holiday', 'cnt', data = bike_new)\n",
    "plt.subplot(133)\n",
    "sns.barplot('workingday', 'cnt', data = bike_new)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* By observing the plots we can came to conclusion that\n",
    "    - There is a increase in number of bike users from year 2018(0) to year 2019(1).\n",
    "    - There are more users during holidays(0) as compared to Non holidays(1).\n",
    "    - There is a very little discrepancy between users of BoomBike on a working day(1) and non-working day(0)."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '>Visualizing Catagorical Variables Variables vs 'cnt' </span> <br>\n",
    "* Build boxplot of all categorical variables (before creating dummies) againt the target variable 'cnt' \n",
    "* To see how each of the predictor variable stackup against the target variable."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 26,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1440x576 with 6 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize=(20, 8))\n",
    "plt.subplot(2,3,1)\n",
    "sns.boxplot(x = 'season', y = 'cnt', data =bike_new)\n",
    "plt.subplot(2,3,2)\n",
    "sns.boxplot(x = 'mnth', y = 'cnt', data =bike_new)\n",
    "plt.subplot(2,3,3)\n",
    "sns.boxplot(x = 'weathersit', y = 'cnt', data = bike_new)\n",
    "plt.subplot(2,3,4)\n",
    "sns.boxplot(x = 'holiday', y = 'cnt', data =bike_new)\n",
    "plt.subplot(2,3,5)\n",
    "sns.boxplot(x = 'weekday', y = 'cnt', data = bike_new)\n",
    "plt.subplot(2,3,6)\n",
    "sns.boxplot(x = 'workingday', y = 'cnt', data = bike_new)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Insights <br>\n",
    "There were 6 categorical variables in the dataset.<br>\n",
    "\n",
    "We used Box plot (refer the fig above) to study their effect on the dependent variable (â€˜cntâ€™) .<br>\n",
    "\n",
    "The inference that We could derive were:<br>\n",
    "\n",
    "* **season**: Almost 32% of the bike booking were happening in season3(fall) with a median of over 5000 booking (for the period of 2 years). This was followed by season2(summer) & season4(winter) with 27% & 25% of total booking. This indicates, season can be a good predictor for the dependent variable.<br>\n",
    "\n",
    "* **mnth**: Almost 10% of the bike booking were happening in the months 5,6,7,8 & 9 with a median of over 4000 booking per month. This indicates, mnth has some trend for bookings and can be a good predictor for the dependent variable.<br>\n",
    "\n",
    "* **weathersit**: Almost 67% of the bike booking were happening during â€˜weathersit1 with a median of close to 5000 booking (for the period of 2 years). This was followed by weathersit2 with 30% of total booking. This indicates, weathersit does show some trend towards the bike bookings can be a good predictor for the dependent variable.<br>\n",
    "\n",
    "* **holiday**: Almost 97.6% of the bike booking were happening when it is not a holiday which means this data is clearly biased. This indicates, holiday CANNOT be a good predictor for the dependent variable.<br>\n",
    "\n",
    "* **weekday**: weekday variable shows very close trend (between 13.5%-14.8% of total booking on all days of the week) having their independent medians between 4000 to 5000 bookings. This variable can have some or no influence towards the predictor. I will let the model decide if this needs to be added or not.<br>\n",
    "\n",
    "* **workingday**: Almost 69% of the bike booking were happening in â€˜workingdayâ€™ with a median of close to 5000 booking (for the period of 2 years). This indicates, workingday can be a good predictor for the dependent variable<br>\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "##  <span style = 'color : Green' >  Correlation Matrix"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "image/png": "iVBORw0KGgoAAAANSUhEUgAABRYAAARiCAYAAADRH6u9AAAAOXRFWHRTb2Z0d2FyZQBNYXRwbG90bGliIHZlcnNpb24zLjMuMiwgaHR0cHM6Ly9tYXRwbG90bGliLm9yZy8vihELAAAACXBIWXMAAAsTAAALEwEAmpwYAAEAAElEQVR4nOzdd5gUVdbH8W91T855yDlnRAXJWVFAgophzWHVNb6uu4Y1B0TBuChiDmtARRETKCA5oxIlZybnPB3q/aNhoJkBppXunhl/n+eZh6muU13nXmo6nLp1yzBNExERERERERERERFPWPydgIiIiIiIiIiIiNQ+KiyKiIiIiIiIiIiIx1RYFBEREREREREREY+psCgiIiIiIiIiIiIeU2FRREREREREREREPKbCooiIiIiIiIiIiHhMhUUREREREREREZFazDCMtw3DSDcMY+MJ1huGYbxsGMYOwzDWG4ZxxunYrwqLIiIiIiIiIiIitdu7wHknWT8CaH345ybgtdOxUxUWRUREREREREREajHTNBcB2ScJuRB433RZAcQYhlH/z+5XhUUREREREREREZG6rSGw/5jlA4cf+1MC/uwTnEpQ9+tMb++jLtj640v+TqHWGPzAHH+nUCu8+++B/k6hVuj47UR/p1BrOErL/Z1CrWAvUT9Vl+l0+juFWqHe6DH+TkHqmNyl8/2dQq2wf/4Gf6dQa3R8+11/p1Ar/N+CTH+nUCvERwT5O4Va47Fz2xn+zqEmqqt1KNuv7/wd1yXMR0w3TXO6B09R1fHyp/vK64VFERERERERERER+eMOFxE9KSQe7wDQ+JjlRsChP5UUuhRaRERERERERESkrvsauOrw3aF7AXmmaab82SfViEUREREREREREZFazDCMj4GBQIJhGAeAR4BAANM0pwHfAecDO4Bi4NrTsV8VFkVEREREREREpE4wLFZ/p+AXpmledor1JvCP071fXQotIiIiIiIiIiIiHlNhUURERERERERERDymwqKIiIiIiIiIiIh4TIVFERERERERERER8Zhu3iIiIiIiIiIiInXCX/XmLf6iEYsiIiIiIiIiIiLiMRUWRURERERERERExGMqLIqIiIiIiIiIiIjHNMeiiIiIiIiIiIjUCZpj0bc0YlFEREREREREREQ8psKiiIiIiIiIiIiIeEyFRREREREREREREfGY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uoXFL4Azjnvsc6DH6U1HRERERERERETkj9Eci7510sKiYRjtgI5AtGEY445ZFQWEeDMxERERERERERERqblONWKxLTASiAFGHfN4AXCjl3ISERERERERERGRGu6khUXTNGcBswzDOMc0zeU+yklERERERERERERquOrOsTjWMIxNQAnwA9AVuMs0zQ+9lpmIiIiIiIiIiIgHDKvmWPQlSzXjhpummY/rsugDQBvgXq9lJSIiIiIiIiIiIjVadQuLgYf/PR/42DTNbC/lIyIiIiIiIiIiIrVAdS+Fnm0Yxu+4LoW+1TCMRKDUe2mJiIiIiIiIiIhITVatEYumad4HnAOcaZqmDSgCLvRmYiIiIiIiIiIiIlJzVXfEIkB7oJlhGMdu8/5pzkdEREREREREROQPMSy6eYsvVauwaBjGB0BL4FfAcfhhExUWRURERERERERE/pKqO2LxTKCDaZqmN5MRERERERERERGR2qG6d4XeCNTzZiIiIiIiIiIiIiJSe1R3xGICsNkwjFVA2ZEHTdMc7ZWsREREREREREREPKQ5Fn2ruoXFR72ZhIiIiIiIiIiIiNQu1Sosmqa50NuJiIiIiIiIiIiISO1x0sKiYRhLTNPsaxhGAa67QFesAkzTNKO8mp2IiIiIiIiIiIjUSCctLJqm2ffwv5G+SUdEREREREREROSPsWiORZ+q7l2hRURERERERERERCqosCgiIiIiIiIiIiIeU2FRREREREREREREPFatu0L/GVt/fMnbu6gT2g67098p1Br7Lwv3dwq1wuCpwf5OoVb4Ma7Y3ynUGgEhQf5OoVYIjo3wdwq1hmHR+c3qKO8wxN8p1AoO0zx1kAAQG1ff3ynUCvOe/Ju/U6g1uuSn+zuFWuGeAS39nUKtYDEMf6cgtZyhORZ9Sp/oRURERERERERExGMqLIqIiIiIiIiIiIjHVFgUERERERERERERj6mwKCIiIiIiIiIiIh7z+s1bREREREREREREfEE3b/EtjVgUERERERERERERj6mwKCIiIiIiIiIiIh5TYVFEREREREREREQ8pjkWRURERERERESkTtAci76lEYsiIiIiIiIiIiLiMRUWRURERERERERExGMqLIqIiIiIiIiIiIjHNMeiiIiIiIiIiIjUCZpj0bc0YlFEREREREREREQ8psKiiIiIiIiIiIiIeEyFRREREREREREREfGY5lgUEREREREREZE6QXMs+pZGLIqIiIiIiIiIiIjHVFgUERERERERERERj6mwKCIiIiIiIiIiIh5TYVFEREREREREREQ8ppu3iIiIiIiIiIhInWBYdfMWX9KIRREREREREREREfGYCosiIiIiIiIiIiLiMRUWRURERERERERExGOaY1FEREREREREROoEw6I5Fn1JIxZFRERERERERETEYyosioiIiIiIiIiIiMdUWBQRERERERERERGPaY5FERERERERERGpEzTHom9pxKKIiIiIiIiIiIh4rM6NWFyzYhmvvTgZp9PJeaPGMOHKa6qM27plE3ffdC33P/40/QYNBeD5px9j5dIlxMTG8vqHM3yYdc0z/ZFrOb9/VzKy8+l+8cP+TserQjqcSdjZrmPAtJVRMHcG9oyDleKiRl5FYL0m4HBgS9lL/txPwOms9n6C23Yjos/5WOOTyf5gMvbU/Uf3f9aQiriApAZkv/cs9vTKOdRkfdslct+YTlgtBl+s2Meb83e4rY8ICWDSFd2pHxuK1WLhnQU7+Wr1foICLLx/W2+CAixYLRbm/naIqXO2+akV3hM1/BJCWnXEtJWTO/t9bIf//49ljYknduz1WELDsaXsI2fWu+B0EBCfTMyoqwis15j8n7+maMVPFdvEjLyS4NadcRYVkDH9CR+2yDuCmrUjYvA4MCyUblhB8aqfKsVEDB5HUPMOYLeR//3/sKcfqNa2oWcOInLgGDKmPoBZUuST9nhLYNO2hPcfg2FYKN20kpK18yvFhPcfQ1Cz9pj2cgp+/ATH4de1iCETCGreHmdJIbn/m1wRb01oQMTgizCsAZhOJ0U/f4E9rfJxWpsENmlLeL/RrmNi8ypK1y2oFBPW70KCmrbDtNsonPcpjoyDWCKiiRh6KUZYJJgmZZtWUrp+CQBBLbsQevYwrHFJ5H32Co7Dx19dsnTpUp57dhJOp5MxY8dy3XXXu61fsGABr706FcOwYA2wcu+999K9+xmUlZVx/XXXUm6z4bDbGTp0GLfcequfWuF9y5YuZfJzz7r6acxYrrnuOrf1Py9YwLTXXsViGFitAdxz77106969Yr3D4eDKKy4nKSmJF19+xdfp+9TiNb8y8bX3cTidXHTeIG6ccKHb+tnzl/DWjK8BCAsN4eHbr6ddi6YAPPj8NBau/IW4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hmGEANcDHYGQI4+bpnmdl/ISERERERERERGRGqy6cyx+ANQDzgUWAo2AAm8lJSIiIiIiIiIiIjVbdQuLrUzTfAgoMk3zPeACoLP30hIREREREREREZGarLo3b7Ed/jfXMIxOQCrQzCsZiYiIiIiIiIiI/AGGbt7iU9UtLE43DCMWeAj4GogAHvZaViIiIiIiIiIiIlKjVfeu0G8e/nUh0MJ76YiIiIiIiIiIiEhtUK05Fg3DSDYM4y3DML4/vNzBMIzrvZuaiIiIiIiIiIiI1FTVvRT6XeAd4MHDy9uAT4G3vJCTiIiIiIiIiIiIxzTHom9V967QCaZpzgCcAKZp2gGH17ISERERERERERGRGq26hcUiwzDiARPAMIxeQJ7XshIREREREREREZEarbqXQv8frrtBtzQMYymQCFzktaxERERERERERESkRqtuYbElMAJoDIwHenqwrYiIiIiIiIiIiNdZDM2x6EvVvRT6IdM084FYYCgwHXjNa1mJiIiIiIiIiIhIjVbdwuKRG7VcAEwzTXMWEOSdlERERERERERERKSmq25h8aBhGK8DlwDfGYYR7MG2IiIiIiIiIiIiUsdUd57ES4DzgMmmaeYahlEfuNd7aYmIiIiIiIiIiHjGsGiORV+qVmHRNM1iYOYxyylAireSEhERERERERERkZpNlzOLiIiIiIiIiIiIx1RYFBEREREREREREY9Vd47FP2zwA3O8vYs6Yf9l4f5OodZo/HGRv1OoFfZfNtffKdQKlsAQf6dQazhtdn+nUCuU5xf7O4VaoyQjx98p1ApBnz3j7xSkjlk/c5m/U6gV+t0xwN8p1BrOkEh/p1ArfL01w98p1AqRwV4vU9QZ18VH+DuFGklzLPqWRiyKiIiIiIiIiIiIx1RYFBEREREREREREY+psCgiIiIiIiIiIiIeU2FRREREREREREREPKZZUUVEREREREREpE6w6OYtPqURiyIiIiIiIiIiIuIxFRZFRERERERERETEYyosioiIiIiIiIiIiMc0x6KIiIiIiIiIiNQJhobQ+ZS6W0RERERERERERDymwqKIiIiIiIiIiIh4TIVFERERERERERER8ZgKiyIiIiIiIiIiUicYhlEnf6rZ9vMMw9hqGMYOwzDuq2J9tGEYsw3D+M0wjE2GYVz7Z/tbhUUREREREREREZFazDAMKzAV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rQy75jZmTLof0+mk84BzSWzUjF9+mg1A96Gj2LpqEb/89A0Wq5WAwCBG3/ZgxXy5trJS9mxcy3nX3+XHVsjxavKNTuoiozo1QsP1VzMcuBY4E5gBvGWa5s5Tbdv8ps9rZBGyplnRaqG/U6g1Gn/817gL85+1/7LwUwcJlsDqnF8RAKfNfuogwVGufqqukoycUwcJST3a+TsFqWN2zKzZI7RriuQzW/o7hVoj8YZ/+juFWuHVnSp2VEdksD6fV9d1ZzbRQVWFc56eVyfrUMsfGFIj/7+rNYv24RGKqYd/7EAs8LlhGM96MTcRERERERERERGpoU55KsAwjDuAq4FM4E3gXtM0bYZhWIDtwL+8m6KIiIiIiIiIiIjUNNUZY5wAjDNNc++xD5qm6TQMY6R30hIREREREREREfGMRXMs+tQpC4umaT58knVbTm86IiIiIiIiIiIiUhtUa45FERERERERERERkWOpsCgiIiIiIiIiIiIe033cRURERERERESkTjAMzbHoSxqxKCIiIiIiIiIiIh5TYVFEREREREREREQ8psKiiIiIiIiIiIiIeExzLIqIiIiIiIiISJ1gaAidT6m7RURERERERERExGMqLIqIiIiIiIiIiIjHVFgUERERERERERERj6mwKCIiIiIiIiIiIh7TzVtERERERERERKROsFgMf6fwl6IRiyIiIiIiIiIiIuIxFRZFRERERERERETEYyosioiIiIiIiIiIiMc0x6KIiIiIiIiIiNQJhuZY9CmNWBQRERERERERERGPqbAoIiIiIiIiIiIiHlNhUURERERERERERDymORZFRERERERERKROMAzNsehLGrEoIiIiIiIiIiIiHlNhUURERERERERERDymwqKIiIiIiIiIiIh4THMsioiIiIiIiIhInWCxaI5FX9KIRREREREREREREfGYCosiIiIiIiIiIiLiMRUWRURERERERERExGMqLIqIiIiIiIiIiIjHvH7zlnf/PdDbu6gTBk8N9ncKtcb+y+b6O4VaofHHRf5OoVZIuT7e3ynUGvbScn+nIHVMUo92/k6hVggbNN7fKdQKpqHz5dXVefDF/k6hVphzzmX+TqHWGNq3r79TqBVu6X6uv1OoHZx2f2cgtZyhm7f4lD6BiYiIiIiIiIiIiMdUWBQRERERERERERGPqbAoIiIiIiIiIiIiHvP6HIsiIiIiIiIiIiK+YNUciz6lEYsiIiIiIiIiIiLiMRUWRURERERERERExGMqLIqIiIiIiIiIiIjHNMeiiIiIiIiIiIjUCZpj0bc0YlFEREREREREREQ8psKiiIiIiIiIiIiIeEyFRREREREREREREfGY5lgUEREREREREZE6QXMs+pZGLIqIiIiIiIiIiIjHVFgUERERERERERERj6mwKCIiIiIiIiIiIh5TYVFEREREREREREQ8ppu3iIiIiIiIiIhInaCbt/iWRiyKiIiIiIiIiIiIx1RYFBEREREREREREY+psCgiIiIiIiIiIiIe0xyLIiIiIiIiIiJSJ2iORd/SiEURERERERERERHxmAqLIiIiIiIiIiIi4jEVFkVERERERERERMRjmmNRRERERERERETqhADNsehTGrEoIiIiIiIiIiIiHlNhUURERERERERERDymwqKIiIiIiIiIiIh4THMsioiIiIiIiIhInWDVHIs+pRGLIiIiIiIiIiIi4jEVFkVERERERERERMRjdfpS6I1rVvDp6y/idDrpe+4oRlxypdv6revXMfXx+0ioVx+AM3oPYOTl1/kjVZ/r2y6R+8Z0wmox+GLFPt6cv8NtfURIAJOu6E792FCsFgvvLNjJV6v3ExRg4f3behMUYMFqsTD3t0NMnbPNT604PUI6nEnY2UMBMG1lFMydgT3jYKW4qBF/I6hxK5xlJQDkf/8h9vTKcScS3LYbEX3OxxqfTPYHk7Gn7j+6/7OGVMQFJDUg+71nPXru2mD6I9dyfv+uZGTn0/3ih/2djk8FNmlLeL/RYFgo3byK0nULKsWE9buQoKbtMO02Cud9iuPwMRg++GKCmnXAWVJI3sdTKuKt8fUJHzQeIzAIZ34OhXM/wrSV+axNp1vUsIsJbtkR01ZO7jcfYE/bXynGGh1PzJjrsISEYUvdT+7s98DpOPX2hkHCNf/GUZhLzmfTAAhp152IvhcQkJBM1rvPYUvd55N2ekvU8EsIaXW4/bPfx5ZaRf/FxBM79nosoeHYUvaRM+tdcDoIiE8mZtRVBNZrTP7PX1O04iffN8BLlu08xOS563CaJmO6teSa3h3c1pumyeS561i68xAhgVYeHdmLdvXjKtY7nE6ufHsOSZFhvDhhQMXjn6zexow12wiwGPRp1YA7h3T3WZt8YfGqX5j46js4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IiIiIiIiIh7TzVt8q7qFxQ+BfwIbOXpXaBEREREREREREfmLqm5hMcM0zdlezURERERERERERERqjeoWFh8xDONNYB7uN2+Z6ZWsREREREREREREpEarbmHxWqAdrhu3HLkU2gRUWBQRERERERERkRrBamiORV+qbmGxq2manb2aiYiIiIiIiIiIiNQalmrGrTAMo4NXMxEREREREREREZFao7ojFvsCVxuGsRvXHIsGYJqm2cVrmYmIiIiIiIiIiEiNVd3C4nlezUJERERERERERORPslo0x6IvVauwaJrmXm8nIiIiIiIiIiIiIrVHdedYFBEREREREREREamgwqKIiIiIiIiIiIh4rLpzLIqIiIiIiIiIiNRommPRtzRiUURERERERERERDymwqKIiIiIiIiIiIh4TIVFERERERERERER8ZgKiyIiIiIiIiIiIuIx3bxFRERERERERETqhADdvMWnNGJRREREREREREREPKbCooiIiIiIiIiIiHhMhUURERERERERERHxmGGapld3kPnyPd7dQR1Rnl/s7xRqjcDwEH+nUCtYAjWFanXUfyvL3ynUGrtGW/2dQq3w3cuL/Z1CrTH8pp7+TqFWsOr1vFpMh9PfKdQa1pAgf6dQK+gzZ/WF1Y/3dwq1wvbPlvg7hVohLCnC3ynUGm2nz9RkglW4/9vNdbIONfGCDjXy/1sjFkVERERERERERMRjKiyKiIiIiIiIiIiIx1RYFBEREREREREREY9p0h4REREREREREakTrJYaORVhnaURiyIiIiIiIiIiIuIxFRZFRERERERERETEYyosioiIiIiIiIiIiMc0x6KIiIiIiIiIiNQJmmPRtzRiUURERERERERERDymwqKIiIiIiIiIiIh4TIVFERERERERERER8ZgKiyIiIiIiIiIiIuIx3bxFRERERERERETqBN28xbc0YlFEREREREREREQ8psKiiIiIiIiIiIiIeEyFRREREREREREREfGY5lgUEREREREREZE6QXMs+pZGLIqIiIiIiIiIiIjHVFgUERERERERERERj6mwKCIiIiIiIiIiIh7THIsiIiIiIiIiIlInaI5F39KIRREREREREREREfGYCosiIiIiIiIiIiLiMRUWRURERERERERExGOaY1FEREREREREROoEzbHoWxqxKCIiIiIiIiIiIh5T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iIiIiIiIiEiLddgRi5ZlPQ9gjLkCGGNZVk3t48eAz8IenYiIiIiIiIiIiLRIoS7e0g5wAUW1j521z4mIiIiIiIiIiLQIdk2xGFGhJhb/Biw1xnxV+/hk4M6wRCQiIiIiIiIiIiItXqirQj9rjPkYOK72qd9blpUXvrBERERERERERESkJTvs4i3GmD61/w4jeOvzjtqfdrXPiYiIiIiIiIiIyI/QkUYs/gq4GnigkdcsYGyTRyQiIiIiIiIiIvI92GyaZDGSjrQq9NW1/46JTDgiIiIiIiIiIiISDQ6bWDTGnHe41y3LeqtpwxEREREREREREZFocKRboc85zGsWoMSiiIiIiIiIiIjIj9CRboW+MlKBiIiIiIiIiIiI/BB2ozkWI+mwq0LvY4xJMcb80xizqPbnAWNMSriDExERERERERERkZYppMQi8AxQDvyk9qcMeDZcQYmIiIiIiIiIiEjLdqQ5FvfpblnW+Qc8/rMxZlkY4hEREREREREREZEoEOqIRbcxZuS+B8aYkwB3eEISERERERERERGRli7UEYvXA88fMK9iMXB5eEISERERERERERE5ejYt3hJRoSYWvwP+AXQHUoFSYDKwIixRiYiIiIiIiIiISIsWamLxXaAEWALsCls0IiIiIiIiIiIiEhVCTSx2sCzrjLBGIiIiIiIiIiIiIlEj1MTiHGPMQMuyVoY1GhERERERERERke/JrikWI+qwiUVjzErAqi13pTFmM1ANGMCyLGtQ+EMUERERERERERGRluZIIxYnRCQKERERERERERERiSqHTSxalrUtUoGIiIiIiIiIiIhI9Ah1jkUREREREREREZEWzWbTJIuRFPbEot/jDfefaBViHHHNHULUCNT4mjuEqODTsReSzRPtzR1C1Oj2nr+5Q4gKO24d19whRI34VFdzhxAVbLH6HjgUViDQ3CFEDW9ZZXOHEBW8ZVXNHULU+PWlzzR3CFHh0U1vNHcIUaH8q3eaOwQROQq25g5AREREREREREREoo8SiyIiIiIiIiIiInLUdG+NiIiIiIiIiIi0CjajORYjSSMWRURERERERERE5KgpsSgiIiIiIiIiIiJHTYlFEREREREREREROWpKLIqIiIiIiIiIiMhR0+ItIiIiIiIiIiLSKti1dktEacSiiIiIiIiIiIiIHDUlFkVEREREREREROSoKbEoIiIiIiIiIiIiR01zLIqIiIiIiIiISKtgM5pkMZI0YlFERERERERERESOmhKLIiIiIiIiIiIictSUWBQREREREREREZGjpjkWRURERERERESkVbDbNMdiJGnEooiIiIiIiIiIiBw1JRZFRERERERERETkqCmxKCIiIiIiIiIiIkdNcyyKiIiIiIiIiEirYDOaYzGSNGJRREREREREREREjpoSiyIiIiIiIiIiInLUlFgUERERERERERGRo6bEooiIiIiIiIiIiBw1Ld4iIiIiIiIiIiKtgl1rt0SURiyKiIiIiIiIiIjIUVNiUURERERERERERI6aEosiIiIiIiIiIiJy1DTHooiIiIiIiIiItAo2o0kWI0kjFkVEREREREREROSoKbEoIiIiIiIiIiIiRy2qb4V2nXo+cd36Y9V4Kfv4RXx7djYoY0vJIPWcKzAJifj27KT0g+kQ8B+2fvIZFxPffQCBqnIKn7237r2cp0wivvtALL8Pf8leyj5+CavaHZnGNoG4Ln1wjj0PjA3PynlULfiiQRnn2POI69oPfDWUffwSvvydIdVNGDEG1ymTKXjkD1juyoi054eK69oX16nng7HhXjGXqvmfNyhzqH3kUHWNI5GUiVdiT0nHX1pE6bvP1O0jMVntcI2/EFu8A8uyKJp+H/h9xPcZRtLx4zE2G9WbVlMx693IbYTvKXncBcR3D26Xkg9ewLdnR4My9pQMUif/FJsjkZq8HZS8/3zdsXfY+saQecXv8FeUUPz6YwA4+gzFOfJsYjJzKHzuPmrytkekneGScuaFJPQcSKDGS/E7z1KT27A99tRMMi64GpOQRE3udoreehr8fhIGHodr5BkAWF4PJR+8RE0j577W7ok7ruSs0YMpKCpj6AW3N3c4ERGOc1Z87yE4TzoLe0YORS/cjy9v/7F4qHNWNJm7JY9/frWcgGUxcUBXLj+ud73XLcvin18tZ86WPBwxdm47YwR9ctKo9vm57tVZeP0B/IEAY3t24JqT+tWr++LC9Tw8eyWfXj+B1MT4SDYrLOZszuWBL5YSCFhMGtyNK07oW+91y7J44IulfLspF0esnTvOPpY+bdLrXvcHAlz23OdkuxL41wWjAXji61W8s3xz3fb5+ckDOal7u8g1Kgzmbs7jgRlLCVgWkwZ14/Lj+9R73bIsHpixjDmbc3HExnD7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MLckvGJsNn5z2lBuen02AcvinIFd6Z6ZwptLg4mK84d256RubZizOZfznvwYR4yd284MJtL2Vrj580f79imL03p3rLuI/cMZI/jnjKX4AhbxMXZuPT0Kt5MVoOLLN0k571qMseFZPT/Yz6y908izYg7eLWuI69KX9Cv/iOXzUv7ZK4etC1D+xas4TzkXY7Nh+XxUfPEaAO7l35A8/iLSLvtd8P1XL8C/Nzfy7W4C1RtW4eg5kJxf3INV46X43f1fqmZcfCPF771AoKKU0i/eIn3KVSSPnURN7g4ql34LgC0pmexr/oCJd4Bl4Tz+VPY8cieW14OJicXRrS8lHxy5rxVtVn30FQPOGsPdG2fhrXLz/JW/qXvtxg+f5YWrfkdZXgFXPP8AjmQnGMOu5d/x8vV/AmDYlDMZff2lBHx+vG4PT134i+ZqSkTE2O386foruOpPfyfgD3De+JPp2bkDr3wYHIF/4dmn8ejLb1NSXs5djzwLgN1u542H7jnc20atcJ3PJw7qyt0fL+TCZz4l1mbjjrOObfy2fZFWxoRyz78xxnzfBVua8lbo1szYQp3uUlr+HIQtQ8Cv7RQKf+03jnJk3d479IqTst+Oi5KaO4SoEZ/aem+zakq22FC/B/5xU/8gdN4yTaMRCm9ZdC4I0xzuvvPT5g4hKjy66Y3mDiEqlH/1TnOHEDVSfnaPMpeN+HJjQavMQ43tkdUi/79D7almGmN+C/QH6u73sSwr+patExERERERERERkR8s1MTiS8CrwATgOuByoGXfrykiIiIiIiIiIj8qdt2CHlGh3n+bYVnW00CNZVmzLMv6KXB8GOMSERERERERERGRFizUEYv7lkDONcacDewGOoQnJBEREREREREREWnpQk0s3mOMSQF+BTwMJAO3hC0qERERERERERERadFCSixalvVB7a+lwJjwhSMiIiIiIiIiIvL92DTHYkQdNrFojHkYOOQy3ZZl3dTkEYmIiIiIiIiIiEiLd6TFWxYBiwEHMAzYUPszBPCHNTIRERERERERERFpsQ47YtGyrOcBjDFXAGMsy6qpffwY8FnYoxMREREREREREZEWKdTFW9oBLqCo9rGz9jkREREREREREZEWwX6ke3OlSYWaWPwbsNQY81Xt45OBP4cnJBEREREREREREWnpQl0V+lljzMfAcbVP/d6yrLzwhSUiIiIiIiIiIiItWUgDRI0xd1mWlWdZ1ruWZb0L5BtjXgpzbCIiIiIiIiIiItJChXordCdjzK2WZd1rjIkHXgeWhDEuERERERERERGRo2IzprlD+FEJdUrLK4GBxphbgfeBryzLujNsUYmIiIiIiIiIiEiLdtgRi8aYYQc8fBB4HPgWmGWMGWZZlkYtioiIiIiIiIiI/Agd6VboBw56XAz0q33eAsaGIygRERERERERERFp2Q6bWLQsa0ykAhEREREREREREZHoEdLiLcaYHOCvQDvLss40xvQDTrAs6+mwRiciIiIiIiIiIhIiuxZviahQF295DvgUaFf7eD3wf2GIR0RERERERERERKJAqInFTMuyXgMCAJZl+QB/2KISERERERERERGRFi3UxGKlMSaD4IItGGOOB0rDFpWIiIiIiIiIiIi0aCHNsQj8EngP6G6M+RbIAqaELSoREREREREREZGjZNMcixEVUmLRsqwlxpiTgd6AAdZZllUT1shERERERERERESkxQrpVmhjTCLwe+D/LMtaBXQxxkwIa2QiIiIiIiIiIiLSYoU6x+KzgBc4ofbxTuCesEQkIiIiIiIiIiIiLV6ocyx2tyxrqjHmIgDLstzG6KZ1ERERERERERFpOeyhDqGTJhHq5vYaYxLYvyp0d6A6bFGJiIiIiIiIiIhIixbqiMU7gE+AjsaYl4CTgCvCFZSIiIiIiIiIiIi0bKEmFi8DPgTeADYDN1uWtTdsUYmIiIiIiIiIiEiLFmpi8VlgJDAO6AYsM8bMtizrwbBFJiIiIiIiIiIichRsWhIkokJKLFqW9aUxZhZwDDAGuA7oDxwxsehze39QgD8W8WnO5g4hanjLqpo7BGlFPnro6+YOIWrsuHVcc4cQFTr+r7K5Q4gam87W+TwUccmJzR1CVLDFhvp9uSRkpzV3CFEhUONr7hCixo0X92/uEKLC3rdeaO4QokJ1SUVzhxA1Upo7ABFCTCwaY2YAScBc4GvgGMuy8sMZmIiIiIiIiIiIiLRcoa4KvQLwAgOAQcCA2lWiRURERERERERE5Eco1FuhbwEwxjiBKwnOudgGiA9faCIiIiIiIiIiItJShXor9I3AKGA4sA14huAt0SIiIiIiIiIiIi2C1m6JrFBnuU4A/gkstixLsxiLiIiIiIiIiIj8yIV6K/R94Q5EREREREREREREokeoi7eIiIiIiIiIiIiI1An1VmgREREREREREZEWzYYmWYwkjVgUERERERERERGRo6bEooiIiIiIiIiIiBw1JRZFRERERERERETkqGmORRERERERERERaRWMpliMKI1YFBERERERERERkaOmxKKIiIiIiIiIiIgcNSUWRURERERERERE5KhpjkUREREREREREWkVbJpjMaI0YlFERERERERERESOmhKLIiIiIiIiIiIictSUWBQREREREREREZGjpsSiiIiIiIiIiIiIHDUt3iIiIiIiIiIiIq2C0eItEaURiyIiIiIiIiIiInLUlFgUERERERERERGRo6bEooiIiIiIiIiIiBw1zbEoIiIiIiIiIiKtgg1NshhJGrEoIiIiIiIiIiIiR02JRRERERERERERETlqSiyKiIiIiIiIiIjIUdMciyIiIiIiIiIi0ioYTbEYURqxKCIiIiIiIiIiIkdNiUURERERERERERE5aq3mVuiUM6bi6DkAq8ZL8TvPUZO3o0EZe2oG6edfjS0hEW/uDorffgYCfmIyckibdAWxbTtS9uW7VMz9vK5Ozs1/waquxrICEAhQ8ORfI9msJhXbuTdJoydjjA3P6vm4F3/ZoEzS6MnEdemL5fNS/vkr+At2AeA8dSpxXfsScFdQ8tL9deXtme1wjp2CscdgBQJUznwT356G2z7aJI//CY4e/bFqvJS8P/2Q+1PauT/DlpBETe52it99rm5/Sj3nMmLbdKRs5ntUzvsCAFtyGmkTL8fmTAbLomrJN1Qu/CrCLQuvcGy31ujEv/2RjuNG43N7mHnDrRSuWNOgzJgn7iNryAACvhoKFq9k9i13YPl8ALQ96VhOuPdWbDExeIpK+GDCtEg3ocnEde2L69Tzwdhwr5hL1fzPG5RxnXo+cd2C+1XZxy/i27PzsHXjew/BedJZ2DNyKHrhfnwH7IcxWe1wjb8QW7wDy7Iomn4f+H2RaWwzeeKOKzlr9GAKisoYesHtzR1OxKWefRGOXgOxarwUvfkMNbnbG5Sxp2WS8ZNra89L2yh84ynw+0kcfByuUWcCYHmrKX7vBWrygvufcSSQPvkKYnPag2VR9PZzeHdsimjbmlI4jkXnKZOI7z4Qy+/DX7KXso9fwqp2R7RdTS22cx+cp5yLsRncq+bjXjijQZmkU84lvmtfrJoayj/7H7784HZy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fTXVoiQSmlBvhd4MVABP1v6UAXuAXrWPRURERERERERE5Eck1MVbhlqWNfqAx+8bY2ZbljXaGLM6HIGJiIiIiIiIiIhIyxXqiMUsY0ynfQ9qf8+sfeht8qhERERERERERESkRQt1xOKvgG+MMZsAA3QFbjDGJAHPhys4ERERERERERERaZlCXRX6I2NMT6APwcTi2gMWbPl3mGITEREREREREREJmU1rt0RUqCMWAYYDXWrrDDLGYFnW9LBEJSIiIiIiIiIiIi1aSIlFY8wLQHdgGeCvfdoClFgUERERERERERH5EQp1xOIIoJ9lWVY4gxEREREREREREZHoEGpicRXQBsgNYywiIiIiIiIiIiLfm9EcixEVamIxE1hjjFkAVO970rKsiWGJSkRERERERERERFq0UBOLd4YzCBEREREREREREYkuISUWLcuaFe5AREREREREREREJHocNrFojPnGsqyRxphygqtA170EWJZlJYc1OhERERERERERkRDZmjuAH5nDJhYtyxpZ+68rMuGIiIiIiIiIiIhINAgpkWuM+Vkjz/2t6cMRERERERERERGRaBDq4i1TjDEey7JeAjDGPAo4wheWiIiIiIiIiIiItGShJhbPA94zxgSAM4Eiy7JuCF9YIiIiIiIiIiIiR8cY09wh/KgcafGW9AMeXgW8A3wL3GWMSbcsq+hIf8AKBH5QgD8WxqbpRUPlLihu7hCiQvbwPs0dQlQYf81xzR1C1IhP1XS7odh0dlVzhxA1un9oHbmQMKftmuYOQVqZebc91dwhRIXLvM82dwhRY9Enm5s7hKjg/WhTc4cQFQqq/c0dQtT44z3NHYHIkUcsLqbhatBn1/5YQLcwxSUiIiIiIiIiIiIt2JFWhe5qjLEBJ1iW9W2EYhIREREREREREZEW7oj331qWFQDuj0AsIiIiIiIiIiIiEiVCXbzlM2PM+cBblmVpQiQREREREREREWlxbFq7JaJCTSz+EkgC/MYYN8G5Fi3LspLDFpmIiIiIiIiIiIi0WCElFi3L0lKgIiIiIiIiIiIiUifUEYsYYyYCo2sfzrQs64PwhCQiIiIiIiIiIiItXUiJRWPM34BjgJdqn7rZGDPSsqzfhy0yERERERERERGRo6ApFiMr1BGLZwFDaleIxhjzPLAUUGJRRERERERERETkR8h2FGVTD/g9pYnjEBERERERERERkSgS6ojFvwJLjDEzCY4qHQ3cGq6gREREREREREREpGULNbF4NvAMUAxsB35nWVZe2KISERERERERERE5SjZNshhRoSYWnwVGAhOBbsAyY8xsy7IeDFtkIiIiIiIiIiIi0mKFlFi0LOtLY8wsgitDjwGuA/oDSiyKiIiIiIiIiIj8CIWUWDTGzACSgLnA18AxlmXlhzMwERERERERERERablCvRV6BTAcGACUAiXGmLmWZbnDFpmIiIiIiIiIiMhRMEaTLEZSqLdC3wJgjHECVxKcc7ENEB++0ERERERERERERKSlCvVW6BuBUQRHLW4juEL012GMS0RERERERERERFqwUG+FTgD+CSy2LMsXxnhEREREREREREQkCoR6K/R94Q5EREREREREREREokeoIxZFRERERERERERaNJvWbokoW3MHICIiIiIiIiIiItFHiUURERERERERERE5akosioiIiIiIiIiIyFHTHIsiIiIiIiIiItIqaIrFyNKIRRERERERERERETlqSiyKiIiIiIiIiIjIUVNiUURERERERERERI6a5lgUEREREREREZFWwWY0y2IkacSiiIiIiIiIiIiIHDUlFkVEREREREREROSoKbEoIiIiIiIiIiIiR63VzLGYcuaFJPQcSKDGS/E7z1KTu71BGXtqJhkXXI1JSKImdztFbz0Nfj8JA4/DNfIMACyvh5IPXqJmz07syWmknfdT7M4UsCwqF8+mYt6MSDetycR26k3SqIlgbHjWLMCz5KsGZRJHTSKucx8sXw0VM17FX7ALmzMF52kXYhJdYFlUr56PZ8U3AMR1H0TCseOwp2dT+vrD+PN3RrpZEZFx3uUk9RtKoKaa/Jf+i3fn1gZlYtKzyLn8ZmxJSXh3bGXPi/8Bvx9Hj360uerX+ArzAahcsYDiT9+KcAvCY86m3dz/2RIClsXkId254sR+9V63LIv7P1vCt5t244i1c+eE4+nTNr3udX8gwLRnPiXblci/p55c9/wrC9fz2qL1xNgMJ/Vox82nDo1Ym8IlfeIlJPQejFXjZe9rT+Ldva1BmZi0TLIuvgFbYhLeXdsoePXx4Dmq31DSxp8PVgArEKDo/Zeo3rphf0VjaPuLP+MvKyb/uX9FsFVNb+6WPP751XIClsXEAV25/Lje9V63LIt/frWcOVvycMTYue2M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Z1bl4S0iIiIiIiIiIiLHPeMEnWPRMAw78BpwCrAX+NMwjO8sy1p/UNgOYJBlWVmGYYwC3gRO+jefqx6LIiIiIiIiIiIidVtvYKtlWdstyyoFPgXGHhxgWdYiy7Ky9r9cDDT6tx+qwqKIiIiIiIiIiMhxzDCMqwzDWHbQv6sOCYkH9hz0eu/+9w7nctwPbf5XNBRaRERERERERETkOGZZ1pu4hy4fjlHZapUGGsYQ3IXFk/9tXiosioiIiIiIiIiI1G17gYSDXjcC9h0aZBhGZ+C/wCjLsjL+7YeqsCgiIiIiIiIiIvXDCfrwFuBPoJVhGM2AROBcYNLBAYZhNAa+Bi60LGvzsfhQFRZFRERERERERETqMMuynIZh3ADMBOzAO5ZlrTMM45r9y6cBDwGRwFTDMACclmX1/Defq8KiiIiIiIiIiIhIHWdZ1k/AT4e8N+2gn68ArjiWn6mnQouIiIiIiIiIiEi1qceiiIiIiIiIiIjUDyfuHIu1QoXF44TLqvQJ4CL/mGWoQ3JVWC790akqy1RbVYXNW39a5dgybPbaTqFOsExXbadQZxg+frWdQp1QkltS2ylIPePSJV+VuEw1lEhdosqDiIiIiIiIiIiIVJsKiyIiIiIiIiIiIlJtGq8lIiIiIiIiIiL1g6ZH8Sj1WBQREREREREREZFqU2FRREREREREREREqk2FRREREREREREREak2zbEoIiIiIiIiIiL1gmWatZ3CCUU9FkVERERERERERKTaVFgUERERERERERGRalNhUURERERERERERKpNhUURERERERERERGpNj28RURERERERERE6gfTVdsZnFDUY1FERERERERERESqTYVFERERERERERERqTYVFkVERERERERERKTaNMeiiIiIiIiIiIjUD5pj0aPUY1FERERERERERESqTYVFERERERERERERqTYVFkVERERERERERKTaNMeiiIiIiIiIiIjUC5ZLcyx6knosioiIiIiIiIiISLWpsCgiIiIiIiIiIiLVpsKiiIiIiIiIiIiIVJvmWBQRERERERERkfrBNGs7gxOKeiyKiIiIiIiIiIhItamwKCIiIiIiIiIiItWmwqKIiIiIiIiIiIhUmwqLIiIiIiIiIiIiUm16eIuIiIiIiIiIiNQPpqu2MzihqMeiiIiIiIiIiIiIVJsKiyIiIiIiIiIiIlJtKiyKiIiIiIiIiIhItdW7ORZ/X7WByR98i8s0OXtIH64cM6zc8u2JKTzwxqes37mXm88ZzaWnDylbdspNjxPo74vNZsPLZuPzJ2/zdPoes2jhQqY8+wymaXLmmeO45LLLyi2f+9tvTHt9KjbDwG734vY776Rrt25ly10uFxeeP4mYmBhefPkVT6dfoxZt28eUWSswLYszu7bgkn7tyy23LIsps1awcNs+/LztPHJ6H9o2iChb7jJNLnxnJjHBAbw4cRAA9369kF0ZuQDklTgI9vXmkytHeW6jPOD3pSuY/No77n1v9HCuPO+scst/mD2Ptz/9FoAAfz8evOUq2rZoRklpKRfd8gClDgcul8mIgX254ZJza2ELalb4GZPwa9MZq7SUjC/fxrFvV4UYe3gUUeddg80/iNJ9u8j4/E1wufBv143QU8aBZWGZLrJ/+B8lu7aAlxexV92L4eUFNjtFa5eRM/tbz2/cMfTH9mSem7MS07IY27k5F/dpW265ZVk8N+cvFm1Pws/bi4dG9aJtXDglThdXf/IbpS4Tl2kxrE0jrjq5AwCbU7KZPGs5JS4XdsPG3SO60+GgfbYu8m7SlqDB4zBsBkVrl1D055wKMYGDx+HbrB2Ww0HerP/hTN0LQNAp5+LbvD1mYT5ZHz5TFu/TqguBfUdij4gh+38v4kzZ47Ht8aSEq68ntNdJmCUl7Hz+GQq3bakQE336WGLPPBu/hvH8de44nLnu43fE4GHETXAfn8yiIna99iJFO7Z7NH9PGNG3A8/dcS52u413vv2dKe/9XG55WHAAbz58Cc0bRVNc4uCqx95j/bZ9ANxw3jAuO3MAhmHwzjfzeeV/Fb+bJ4o3H76U0QO7kJaZS7cJD9V2OrVqwepNTP7oO1ymxdmDenHFGUPKLd++L5UH3/qC9bsSuWn8qVw6elC55S7TZOJDrxATHsLU2y/1ZOoe1+a+O4ka2B9XcTHr7nuEvPUbK8QkTDqHxhdNIqBJAnP7DsORnQ2AV1AQHZ95HL8GcRhedna98yH7vvnew1vgGVtWLuXHd1/FMl30GHYaA8dNKrd8x7q/+PjpBwiPiQOg/UkDGDLh4rLlpsvF6/dcQ0hEFBfe+5RHc/cktVPVJG9Ywcqv/4tlmTTvcwpth59dISZ1yxr++uZtLNOFT2AIQ258EpejlN9euR/T6cAyXTTq0o8Oo86rhS2QQ1maY9Gj6lVh0WWaPPnu17x17zXERoYy8YEXGNK9Ay0bxZXFhAYFcO/F4/h12dpKf8e7919HeEiQp1KuFS6Xi6cnP8Vrr08jNjaWi84/n4GDBtG8RYuymN4nncSgwYMxDIMtmzdzz9138dU335Yt/98nn9CsWTMKCgpqYQtqjss0efrn5bw2aQixIf5c9M4sBraKp3l0aFnMwm1J7MnM45trT2ftvgye+nkZ7186omz5//7cTLOoUApKHGXvPXVW/7KfX5i9giBfH89skIe4XC6efPkt3nrmYWKjI5l43V0M6duLlk0TymLiG8Ty3guPExocxO9LVvDI89P49LWn8fH25p3nHiXQ3x+H08mFN9/PgN7d6NK+TS1u0bHl16YzXpGxJE25B5+E5kSceSEpU5+oEBc2cgJ5C2ZRuHop4WdeRFDPgeQv+Y3ibesp2rASAO+4RkSddx1JL9wHTiep/30Gq7QEbHZir7mXok2rKd1TNwsdLtPimdkrePWcgcQEB3DxB7MZ0LIhzaNCymIWbU9mT1Y+X105irVJmTz9ywrevXAYPnYbU88dTICPF06XyZWf/Ebf5nF0ahjJK/NWc0X/9vRr3oCF25J4Ze5qpp03uPY29N8yDIKHnk3219Mw87IJn3QrpdvW4spMKQvxadoOr7BoMt/9D15xTQgaOp7sT18EoGT9UopXLSD41PIXF66MJHK/f4egYed4cms8KrRnb/ziG7H2iosIbNOOxjfczMZbb6gQl79+HTlLF9Pm6efLvV+SksSmu2/FlZ9PSM/eNLnptkrXr8tsNoOX7pnE6OteYG9KFos+vJ8f5q1i446kspi7LxvNqk17OOeOqbRpGsdLd09i5LXP075FQy47cwD9L/4PpQ4nP7xyMzMWrGHrntRa3KLa88H3C5n62RzeffyK2k6lVrlMkyc++Ja37rqCuIhQJj78KkO6t6dFfGxZTGhQAPdcOIZfl6+r9Hd8NHMBzRvGkF9U7Km0a0XUwP4ENElg4cgzCe3SkXYP3cvScy+uEJe9chVpc3+n5wdvlnu/0aQJ5G/bzl/X3Yp3eBj9f/qapB9mYDmcntoEjzBdLr5/+yUuefBZQiKimXbvNbTt2Y+YhKbl4pq063TYYtgfP31FdHxjSooKPZBx7VA7VY1luljx5RsMvPZRAsIimf38nTTs2JuQuAPXMaWF+e6Yax4mIDya4rxsAGxe3gy+/jG8fP0xXU5+e+le4tp1J7Jp/bmOEamKejUUes3W3STERpEQG4mPlxej+3bjt+XlC4iRocF0atEYL3u92vRqWbd2LQkJCTRq1Ahvb29GnHoq8+bOLRcTEBCAYRgAFBUVlf0MkJKSwsIFv3PmuPI90uqDdfsySYgIolF4EN52OyPaN2be5r3lYuZt3svozk0xDINO8VHkFZeSnlcEQEpuIQu37uPMrs0r/f2WZTF7/R5O7dCkxrfFk9Zs3EpCfAMSGsbh4+3N6CEn89uipeViunVoS2iwu2jfuX1rUtIyADAMg0B/fwCcThdOp7Pc960+8G/XjYKViwAo3bMdm18AtuDQCnF+LdpRuHYZAAUrFuLfvjuAu3C4n+HjC1hlr/9eZtjtGLa6fa9oXVImjcKCiA8LwttuY0S7BOZvTSwXM3/rPkZ3aOLe/xpGuve/fPcxKsDHvf1O08TpMjn4W1RQ4r6oyi9xEBXk56lNqhFecY1xZadj5mSA6aJ400p8WnQsF+PToiPFG/4EwJm8C8PXH1ugu0DrSNyOWVzxppArMxVXVlrNb0AtCuvTn4w5swAo2LQBr8AgvMMr9l4t2r6V0tSUCu8XbFiPKz/f/fPG9fhERtdswrWgV4dmbNuTxo7EdBxOF5/P+pMzBnctF9OueQN++3MDAJt2JtOkYSQxEcG0bdaAJWu3U1RcistlMn/FZsYO6VbJp5wYFqzYTFZO/boB+0+s2baHxjGRJMRE4u3lxag+Xfh1xfpyMZEhQXRqnoCX3V5h/eTMbOav2sjZg3t5KuVaEz10EEnTfwQgZ9VavEKC8ImOqhCXt2ETxfuSKryPBV6BgQDYAwJw5ORiOetfr529WzcSGdeQiNiGeHl706n/UDYsW1jl9XMy0ti8YjE9h51Wg1nWPrVT1WTu2kJQVAOCouKweXmT0O1kEtcsKReze8V8GnXuS0C4++++X3AY4L6O8fJ1X8eYLhem6QLq13WMSFVU+SrUMIzuwMm4r2gXWpa1osay+odSsnJoEBlW9jo2IozVWysONzwcwzC4cvIbGBhMGNaXc4b1rYEsa19qaiqxsQd6ccbExrJ27ZoKcb/9+iuvvvIyWZmZ5YY7P/fss9x08y0UFNa/k+XUvEJigwPKXseEBLA2MaNcTFpeEXEhgWWvY0MCSM0rJCrYn+d+WcFNQ7tSUOqgMiv3pBER6EfjiOCa2YBakpKeQYPoyLLXsdGRrN5QcXjh376eMZsBvcsPrZ9w7Z3sTkzmvLEj6dyudY3m62leoWEUZmeWvXblZOEVEk5pXk7Ze7aAIMziQjDNshh7SFjZcv/23Qk7dTy2oGDS3n/xwC83DOJueASvyBjyF/9aZ3srAqTlF5Xf/4IDWLev/P6XmldEbEj5mNS8IqKC/HGZFhd98At7s/IZ360lHRu6v5O3DevKTZ/P56W5q7Asi/+eP9QzG1RDbEFhuPbfKQcw83Pwjmt8SEzoITHZ2IJCMQtyPZTl8ck7KorStAPF09L0NLyjonBkZR5hrcpFjRhFzvKlRw+sYxrGhLEn5UB7JKZk0btjs3Ixqzfv5cwh3Vn011Z6dmhK47hI4mPCWb81kceuG0dEaCBFJQ5G9u/EivVVPw+T+ik1K4e4cufnoazZtrvK6z/98ffcNnE0BcUlRw+u43xjYyhOPnBTozg5Fb+YaErT0qu0/p6PP6Pr1BcYOH8m9oAA1tx+L1jW0VesY3Iz0wmNjCl7HRoRzd4tGyrE7dm8nlfvuJyQ8ChOvegaYhPcx7Kf3n2VERdcTWlxkcdyrg1qp6opyskkIPxAAT8gLJKMXeWvY/JT92GaLua+cj+OkiJaDTyDpr3dUzpYpotfptxOfnoyLU8eRWTT+nUdI1IVVeq2ZxjGQ8D7QCQQBbxrGMYDR4i/yjCMZYZhLHvr658PF3bsVfKHszo9nz565Ea+/M/tTLv7Sv73ywKWbdh2LLM7jlTSTpXcWRkydChfffMtU55/gWlTpwLw+/z5RESE0659+wrx9dWhX6HKzs8Mw+D3LYlEBPjS7ghzt81ct4tTOzQ+7PL65HC73pKVa/h6xhxuu/Kisvfsdjtfv/k8v372Fms2bmXLjvp2IVqxMaxDv0hHOVYVrV9B0gv3kf7hK4SdMu7gX0TyKw+TOPk2fBo1wzs2/lgkXCsqtAlU0i6HP87bbQYfXzKCH649nfVJmWxLcxduv1q5jVuHduWHa0/nlqFdeeLnZcc69dpXoVkq+T7Vw4vLY+IftEtw565EjRjF3nfeqoGEaldl502HNtGz780gLCSApZ88xHUTh/LXpj04XSYbdyYz5f2f+WnqrXz/ys2s2bwXp6v+9ZaS6qlsD6vsvLMyc1duICI4iA7NGh3bpI5Xle5/VT9GRZ7cl7yNm5g/8FQWn3UebR+4C3tg4NFXrHOOfr7QoFkrbp/6KTdMeZs+o8bxyTMPArBp+R8EhYYR3+JEGKqqdqoKq9Jzy/KvTdMka882Tr7qQQZe8wgbZn1OXqp7VI1hszPirhc5/ZH/krl7CzlJ9e06po4yzfr57zhV1R6L5wHdLMsqBjAMYzKwAqg4SRhgWdabwJsAzuU/euxKJjYijKSM7LLXKZnZxISHHH6FQ8SEu4cmRoYGM7xnJ9Zs203Pdi2OslbdExMTS0pKctnr1JQUoqMPP5yre48e7N27h+ysLFb99Rfz581j4YIFlJaWkl9QwIP338fjT/7HE6nXuJjgAFLyDswhkppbSHSQf/mYEH+ScwsAd5ul7I+Zs3E387cksnBbEqVOF/klDh6cvojHx/YD3MMzf9u0hw8vG+mx7fGU2KhIktIO9CxLScsgJrJigXXTtp08/NxUpj31IGGhFXtthgQF0rtrBxb8uZJWzer2cPGgPkMJ6uWefL507w7sYRGw/zzDHhperjcZgFmQh80vAGw2ME13TG75GICSnZvxiohx93AszC973youonjHJvxad8KRklhhvbqgwv6XV0j0IcOWY4IDSMk9ckywnw/dG0fzx45kWkSH8uPandw+rCsAw9s04j91vLBo5mdj3z8EB/b3TizIqTTGWRYTdsL2Vow+fSzRp44GoGDLJnwO+nvnExWNIyPjcKtWyr9pc5rcfDtbHroXV179a9PElCwSYg8cv+Njw9mXnl0uJq+gmKsefa/s9abvn2LnPnePqvemL+C96QsAeOz6cSSmZtV4znJ8iw0PJbnc+XkO0VU8P1+5ZSdzV67n99WbKHE4KCgq4e5pn/L0NfXnIW+NJk2g0Xj3DcOctevxizsw96RfXAwlVeytCNDwrDHsfOtdAIp276Vo7z4Cmzcld03lc1fWVSER0eRkHJi7NSczjeCIyHIxfgEHCqqtu/fh+/++SEFuDrs2rmXjskVsXrkEZ2kpJUWFfPHyk0y46X6P5e8paqeqCQiNpDDrwH5WmJ2BX0j565iAsEh8A4Px8vXDy9ePqBbtyd63k+CYAzf0fQKCiG7ZkeQNKwltULevY0Sqq6oTDe4EDr5y8wWOu+58HVsksDs5jb2pGZQ6nfz0x0qG9Oh49BWBwuISCvZPCF1YXMKiNZtpmRB3lLXqpvYdOrBn924SExNxOBzMmjmTgYPLP31vz+7dZXdIN27YgMPhIDQsjBtuuomfZs7i+59m8OTkyfTq1aveFBUB2jeMYE9mHonZ+ThcLmat383A1uXvkg9qFc9Pq3diWRZrEtMJ8vUmKtifG4Z05aebzuT7G8bw5Lh+9GoaW1ZUBFi6I5mmkSHlhnHWFx3btmR3YhJ7k1IodTj46bcFDOlXfi6kfSlp3PzIMzx17800TWhY9n5mdg65+e5h9cUlJfyxfDXNEup+z4T8xb+S/MrDJL/yMIXrVxDYzf1d8ElojllchJmXU2Gdku0bCejYE4DA7v0p2uCeccLroGEs3g2bgN0LszAfW2Awhp+78G14eePXoj2OtErmXKoj2jcIZ09WPonZBThcJrM27GFAy4blYga0bMhP63a59799Ge79L8ifrMIS8opLASh2uFi6K5Um+6cciA7yZ8Ue9/DXP3enkhBetx/Q5Uzegz08GltIBNjs+LXpRun28heNpdvX4dfOvQ96xTXBKi06YQuLaT9MZ/2NV7P+xqvJ/mMhkcPcD9sKbNMOV0FBtYZB+0TH0OKBR9gx5SlKEvcefYU6aNn6nbRMiKFpwyi8veycM6IXP8xbVS4mNMgfby/3XHiXjRvAghVbyCtwn0NFh7v3u4S4CM4c2o3Pfq5/w8Wlejo2b8TulAz2pmXicDqZsXgVQ7q1q9K6t54zijkv3c+s5+/h2esm0btdi3pVVATY+8kXLD5rEovPmkTanLk0GOuezy60S0eceflVHgYNUJyUTESf3gD4REYQ0KwJRXvq5s3GI4lv2ZaMpESyUpJwOhysWfgrbXv2KxeTl5VZdi2zd8sGLNMiIDiEEedfyZ1vfMHtUz/lnFsfolnHbvWyWAZqp6oKb9yK/PQkCjJSMJ0O9qxcQMOOvcvFNOzYm/Tt6zFdLpylJWTu2kJIbCNK8nMo3X+j31VaQurmVQTX4dFDIv9UVXsslgDrDMP4BXef6lOABYZhvAxgWdZNNZRftXjZ7dx/yVlcNflNTNNk3ODetGwUx2ez3Q9NmDi8H2nZuUx84AXyi4qxGQYf/jyf7565m6y8Am564R0AXC6T0/p3Z0CXqp301DVeXl7cefc93HjdtbhMkzFjx9KiRUu+/OILAMZPmMCcOXP46Yfv8fLywtfXj6eefqbePVCjMl42G3ee2pMb/zcXl2kxpktzWkSH8uVy9zwb43u0on/LhizclsSZU3/Az9vOw6efVKXfPWv9bka0r593r7zsdu6/8Qquuvsx9743ahgtmzbms+9nAjDxjFOZ9uHn5OTm8fhLb5at8/nrz5KWkcV9z7yC6TIxLZNTB/VncN+etbk5x1zxptX4t+lMgzuexnKUkvnl22XLoi+5lcyv3sWVl03WjC+IOu8aQkechWPfbrL+/B0A/w49CezeD1wuLGcpGf97HQB7cCiRE64AwwaGQeGaPyneuKrSHOoCL5uNO4d346Yv5mNaFmd0akaLqFC+Wum+j3V2txb0bx7Hou1JnPXWDPy87Dw4yl08S88v4tGf/sS0LEzLYnibhLKi5H0je/L8nJU4TQtfLzv3nlrHv1+WSf6vXxF61tUYho3idUtwZSTj19l9sVC8ehGlO9bj07QdEZfej+UsJW/Wp2WrB4+6EO+Eltj8Aom44mEK//iZ4nVL8GnRiaAhZ2HzDyJ07JU40xLJ+eaN2trKGpHz5xJCe51Ex7c/xCwpZucLz5Yta/Xof9j50nM4MjOIGTOOuPET8Q6PoP1rb5GzbCm7XnqOBpMuxCs4hCbX3Qy451XacPN1tbU5NcLlMrnlmU/44dVbsNsN3pu+kA3b93Hl2e4bkG99NY+2zRrwzmOX4TJNNmxP4urH3i9b/9NnryUyNBCH08XNkz8hO6/+Pkn0aD586moG9mhDVFgQ23+ewmPTpvPet7/Xdloe52W3c99FY7n6mbdxWSbjBvZyn5//uhiAiUP7kJ6dx8SHXya/qASbzeCjmQuYPvl2gvzr9sO2qit93gKiBvan/8zpuIqLWX/fI2XLur3xEusfeJyStHQSLjiXppdfhE9UJH2nf0r6/IWsf/Bxdkx9iw5PPUqf6Z9hGLDluZdxZGfX2vbUFLvdzumX38T7T96FaZp0HzKK2IRmLJ31HQC9R4xh3eJ5LJ01HZvdjrePL+fc+uAJcS1zMLVT1djsdrqdfSXzpz2KZbpodtJwQhs0ZttC95RuLfqPJCQugbh23Zn1zM0Yho1mfYYT2qAJ2ft28ufHL2GZJpZlkdC1Pw071P8HTYkcyqjKvB2GYVx8pOWWZb1/uGWeHApdlxW1q9sPE/CoL5+u7QzqBP+hE2o7hTph32tTajuFOiO0pe7AVkVpbv17sFVN2fXL6tpOoU7on9K8tlOoEyxT8zlWVcG0MbWdQp0w95LHazuFOiPrs+9rOwWpR1YnVhzZI5V7YlS7E6sSXEWlf3xVL+tQPn3PPi7/f1epx+KRCociIiIiIiIiIiJy4qnqU6FPNwxjpWEYmYZh5BqGkWcYxok5WZOIiIiIiIiIiIhUeY7FF4GzgDVWVcZOi4iIiIiIiIiISL1W1adC7wHWqqgoIiIiIiIiIiIiUPUei3cBPxmGMQ/3E6IBsCzr+RrJSkREREREREREpLr0QDePqmph8UkgH/ADfGouHREREREREREREakLqlpYjLAsa0SNZiIiIiIiIiIiIiJ1RlXnWJxtGIYKiyIiIiIiIiIiIgJUvcfi9cBdhmGUAqWAAViWZYXUWGYiIiIiIiIiIiLVYZq1ncEJpUqFRcuygms6EREREREREREREak7qjQU2nC7wDCMB/e/TjAMo3fNpiYiIiIiIiIiIiLHq6rOsTgV6AtM2v86H3itRjISERERERERERGR415V51g8ybKs7oZhrASwLCvLMAyfGsxLRERERERERESkWiyXq7ZTOKFUtceiwzAMO2ABGIYRDWg2TBERERERERERkRNUVQuLLwPfADGGYTwJLACeqrGsRERERERERERE5LhW1adCf2wYxnJgGGAAZ1qWtaFGMxMREREREREREZHjVpUKi4ZhfGhZ1oXAxkreExERERERERERqX2m5lj0pKoOhe5w8AvDMLyAHsc+HREREREREREREakLjlhYNAzjXsMw8oDOhmHk/v0PSAGmeyRDEREREREREREROe4ccSi0ZVlPAU8ZhvEU8AzQGvD7e3EN5yYiIiIiIiIiIiLHqSrNsQhsB+YDjYC/gD7AH8DQmklLREREREREREREjmdVLSzeBPQCFluWNcQwjLbAozWXloiIiIiIiIiISDXp4S0eVdWHtxRbllUMYBiGr2VZG4E2NZeWiIiIiIiIiIiIHM+q2mNxr2EYYcC3wC+GYWQB+2oqKRERERERERERETm+VamwaFnWuP0/PmIYxm9AKPBzjWUlIiIiIiIiIiIix7Wq9lgsY1nWvJpIRERERERERERE5N+wTLO2UzihVHWORREREREREREREZEyKiyKiIiIiIiIiIhItamwKCIiIiIiIiIiItVW7TkWRUREREREREREjkumq7YzOKGox6KIiIiIiIiIiIhUW433WMxe+GtNf0S9EB7RoLZTqDNWf72otlOoEzoNnVDbKdQJdj+f2k6hzijNLajtFOoE/5jw2k6hzlj84H9rO4U6Ic+5pLZTqBMMH7/aTqHOCLzmu9pOoU54M7WwtlOoM/q8fF1tp1AnOIsctZ1CndDGoSf6VtmoGbWdgYh6LIqIiIiIiIiIiEj1aY5FERERERERERGpHzTHokepx6KIiIiIiIiIiIhUmwqLIiIiIiIiIiIiUm0qLIqIiIiIiIiIiEi1qbAoIiIiIiIiIiIi1aaHt4iIiIiIiIiISL1gmWZtp3BCUY9FERERERERERERqTYVFkVERERERERERKTaVFgUERERERERERGRatMciyIiIiIiIiIiUj+YrtrO4ISiHosiIiIiIiIiIiJSbSosioiIiIiIiIiISLWpsCgiIiIiIiIiIiLVpjkWRURERERERESkftAcix6lHosiIiIiIiIiIiJSbSosioiIiIiIiIiISLWpsCgiIiIiIiIiIiLVpjkWRURERERERESkXrBcmmPRk9RjUURERERERERERKpNhUURERERERERERGpNhUWRUREREREREREpNpUWBQREREREREREZFq08NbRERERERERESkfjDN2s7ghKIeiyIiIiIiIiIiIlJtKiyKiIiIiIiIiIhItamwKCIiIiIiIiIiItWmORZFRERERERERKR+MF21ncEJRT0WRUREREREREREpNpUWBQREREREREREZFqU2FRREREREREREREqq1ezLHo3aQNgQPPxDBsFK9bQtHyXyvEBA48E5+m7bCcpeT98imutEQAgoZNxKdZO8yifLI/nlJuHb/OJ+PXpT+YJqU7N1C48AePbI8n/L7sL556/QNcpsn4kUO4cuLYcsu//3UBb3/+HQAB/n48dOPltG3eBID7n5/GvCUriQgL4bs3nvV47rUh/orrCO3RC7OkhF0vT6Fo+9YKMVGjxxBzxjh8G8Sz+sLxuPJyAfCNT6DJjbfj36IlSR+9R+r0Lz2dvkf8vnQFk197B5dpcvbo4Vx53lnllv8wex5vf/ot4P5OPXjLVbRt0Yyk1HTunfwyGVlZGIaNCaedwoVnn14LW1CzQkdOxK9VRyxHKVnfvocjeU+FGHtYJBFnX4nNP4DSpD1kffMOmC68ImMJH3sJ3g0SyP11Ovl//FK2juHrT/iYC/GK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IiIiIiIiclzTw1s8q6oPbxEREREREREREREpo8KiiIiIiIiIiIiIVJsKiyIiIiIiIiIiIlJtmmNRRERERERERETqBdPlqu0UTijqsSgiIiIiIiIiIiLVpsKiiIiIiIiIiIiIVJsKiyIiIiIiIiIiIlJtmmNRRERERERERETqBcs0azuFE4p6LIqIiIiIiIiIiEi1qbAoIiIiIiIiIiIi1abCooiIiIiIiIiIiFSb5lgUEREREREREZF6wXJpjkVPUo9FERERERERERERqbYa77G459c1Nf0R9cKcJy6o7RTqjAE3DartFOqEmX3Pq+0U6oReNw+p7RTqjNLcwtpOoU4wHc7aTqHOuKj03dpOoU5YPG12badQJ5TkltR2CnXGm6k6nlfFVY371nYKdcbM5etqO4U6IS+5oLZTqBPyneptVlVdajsBEdRjUURERERERERERP4BFRZFRERERERERESk2vTwFhERERERERERqRf08BbPUo9FERERERERERERqTYVFkVEREREREREROo4wzBGGoaxyTCMrYZh3FPJcsMwjJf3L19tGEb3f/uZKiyKiIiIiIiIiIjUYYZh2IHXgFFAe+A8wzDaHxI2Cmi1/99VwOv/9nM1x6KIiIiIiIiIiNQLlnnCzrHYG9hqWdZ2AMMwPgXGAusPihkLfGBZlgUsNgwjzDCMBpZlJf3TD1WPRRERERERERERkeOYYRhXGYax7KB/Vx0SEg/sOej13v3vVTemWtRjUURERERERERE5DhmWdabwJtHCDEqW+0fxFSLeiyKiIiIiIiIiIjUbXuBhINeNwL2/YOYalGPRRERERERERERqRdM1wk7x+KfQCvDMJoBicC5wKRDYr4Dbtg//+JJQM6/mV8RVFgUERERERERERGp0yzLchqGcQMwE7AD71iWtc4wjGv2L58G/ASMBrYChcCl//ZzVVgUERERERERERGp4yzL+gl38fDg96Yd9LMFXH8sP1NzLIqIiIiIiIiIiEi1qceiiIiIiIiIiIjUC9aJO8dirVCPRREREREREREREak2FRZFRERERERERESk2lRYFBERERERERERkWrTHIsiIiIiIiIiIlIvaI5Fz1KPRREREREREREREak2FRZFRERERERERESk2lRYFBERERERERERkWpTYVFERERERERERESqTQ9vERERERERERGR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BtKttRuBtsCpQJ61dqNnmoiIiIiIiIiIiBxEvG2x+DXQ1xjTAXgX+A74FDjRV4GJiIiIiIiIiIjUhUtjLPqVty0WXdbaCuAM4CVr7a1Aiu/CEhERERERERERkfrM28RiuTHmfOASYKJnWohvQhIREREREREREZH6ztvE4uXAUcAT1tr1xpi2wMe+C0tERERERERERETqM6/GWLTWLgNuqvJ+PfC0r4ISERERERERERGpK+vSGIv+tN/EojFmMWD3Nd9a2/0fj0hERERERERERETqvQO1WDzZ8+/1nn8/8vx7IVDkk4hERERERERERESk3ttvYtFauxHAGHOMtfaYKrPuNsZMBx71ZXAiIiIiIiIiIiJSP3k1xiLQzBjT31o7DcAYczTQzHdhiYiIiIiIiIiI1I11aoxFf/I2sfgv4D1jTLTnfS7wfz6JSEREREREREREROo9b58KPQ/oYYyJAoy1Ns+3YYmIiIiIiIiIiEh95lVi0RjTFDgTaAMEG2MAsNYecIzFw94b999HdxDpnp8R6BAaDFdoZKBDaBCG9e8f6BAahPKtawMdQoNx+0XvBTqEBuGGCw4LdAgNxtyf1gU6hAYhfPKkQIcgjUy/V64LdAgNws/zlgY6hAbjeHTs88a6pa8EOoQG4dHf1gQ6hAbjgkAHIIL3XaG/BfKAeUCp78IRERERERERERGRhsDbxGILa+0JPo1ERERERERERETkf2CdNtAhHFSCvCw3wxjTzaeRiIiIiIiIiIiISIPhbYvF/sBlxpj1uLtCG8Baa7v7LDIRERERERERERGpt7xNLI7waRQiIiIiIiIiIiLSoOw3sWiMibLW5gMFfopHRERERERERETkv+JyugIdwkHlQC0WPwVOxv00aIu7C/RuFmjno7hERERERERERESkHttvYtFae7Ln5TRgKvCntXaFz6MSERERERERERGRes3bp0K/D6QArxpj1hpjvjLG3OzDuERERERERERERKQe8+rhLdba340xfwCHA4OBa4CuwMs+jE1ERERERERERMRr1mUDHcJBxavEojFmEtAMmAn8CRxurc3wZWAiIiIiIiIiIiJSf3nbFXoRUIa7lWJ3oKsxJsxnUYmIiIiIiIiIiEi95m1X6FsBjDERwOW4x1xsDjT1XWgiIiIiIiIiIiJSX3nbFfoGYADQB9gIvIe7S7SIiIiIiIiIiEi94HJqjEV/8iqxCIQBLwDzrLUVPoxHREREREREREREGgBvu0I/5+tAREREREREREREpOHw9uEtIiIiIiIiIiIiIpWUWBQREREREREREZE683aMRRERERERERERkXrNOl2BDuGgohaLIiIiIiIiIiIiUmdKLIqIiIiIiIiIiEidKbEoIiIiIiIiIiIidaYxFkVEREREREREpFGwThvoEA4qarEoIiIiIiIiIiIidabEooiIiIiIiIiIiNSZEosiIiIiIiIiIiJSZxpjUUREREREREREGgWXxlj0K7VYFBERERERERERkTpTYlFERERERERERETqTIlFERERERERERERqTONsSgiIiIiIiIiIo2CdboCHcJBpdElFqfNnM0zL72G0+nkjJEnccUlF1SbP/HnX3nvo38DEB4WxgN33kLnjh0A+PCzLxk/4XuMMXRs347H7ruLpk2b+H0d/OHPOQt56s1xOJ0uzhoxhCvPPa3a/Am//8m7X3wHQHhoKA/e+C+6tG8DwH3Pv8Eff80nLiaK78Y+7+fI/W/aX3N5+tW3cLpcnHnS8Vxx4TnV5k/8dTLvfvol4NmmbrueLh3aATD83MtoFhZGkMOBwxHEF2Nf8Xv8/jJtyRqe+eJnXC4XZ/Tvxb9O6F9t/vr0LB4Y9y3LN6dz46mDuWz40QCkZ+dx3/vfkJW/iyBjOHNAby4aemQgVsFnZqzZxuif5+J0WU7r1YHL+x9Wbb61lud+nsf01VsJDQnm4VOP4pCUOABOfvkbwpsG4zBBOIIMH185otqyH85Yxsu/LeC3288kNjzUb+vkL+e8/BBdTxxMWVExH1x2O5sXLK1R5uJ3nqF13+5gIGPVej647HZKdxXR6dh+XPvtWLLWbwFgwfif+OGxxvkbTLnkaiJ69MWWlbLlrRcp2bC2RpmQxGRa3nAXjogISjasZcuY57HOCoLCI2hx1c00SU7BVV7G1rEvU7plYwDWwj+OeOpeWgwbSEVxCdNuuJfsRctqlBnw5rMk9OqKq7yCrPmLmHHbw9iKCpofczhDPn6dwo3ubWrjxN/4e/QYP6+B761eMJvv338N63LSZ+hJDDy9+rnU+qUL+eSZ+4lNag7AoUcOYPDZl1bOdzmdvHH3NUTFJXDxPU/5NXZ/Uj0dWNL5VxDRrQ+uslK2v/cKpZvW1SgTkpBE6lW342gWQcmmdWx75yVwVhB3/GlEHXksAMYRRJOUFqy+9VJcuwpp//RYnCXF4HJhXU42Pn67n9fMdzreeRtxxxyNq6SE5Q89RuGKlTXKpJ17Fi0uOI/wVi2ZNng45bl5ALS85CKSTzweAONw0KxtG6YNOYGK/Hy/rkOgjX3ock4c2IPM7Hx6nf1goMMJqNkzZ/D6S6NxOZ2cOPI0zr/k8mrzp0+dwvtj3yAoKAiHw8F1t4yiW49eAHz9+af88N03WGs5aeTpnHneBbV9RaNwWPNIzu2ZRpAxTFu/k59WZNQo0ykxgnN7puEIgsJSJ6OnrCE5silX9WtTWSYhognfLUln0upMP0YvEniNKrHodDp54vmXGfvyczRPSuS8/7uGwQOOpn3bNpVlWqSk8P6Yl4iOiuTPmX/xyNPP8+m7b7AjI5NPvxzPN5+OIzS0KaPue5gff/ud0046IXAr5CNOp4vHX3+Pd566j+SEeM698R4G9+tLh9YtKsu0SE7ig+ceIjoygqlzFvDQy2/z+StPAHD68GO5cOTx3P3c64FaBb9xOp08/tIY3n7+CZonJnDu1bcw+Jh+tG/TqrJMWkoy4155hujISP6cNYdHRr/CZ2++VDn/vZeeJjYmOgDR+4/T5eLJz35k7C0XkRwbxflPvcOg7p1pn5pYWSYqPIy7zzuB3xdWP0F2OIIYdfZwDm2Vwq6SUs574m2OOqRdtWUbMqfLxdM/zmHMRUNIjgrn4nd+4tjOLWiXuGebmL5mG5t35vPNDSNZsnUnT30/mw+v2LPveeuSYbUmDdPzdvHXunSaR4f7ZV38reuIQSR1bMuDHQfR9sheXPDGEzzT77Qa5b689TFKCgoBOOv5+xl0w6X8/MwbAKz+cw5jTvmXP8P2u4gefWnSPJXVo64krENnUi+/nnUP3VajXPPzLmfnj9+QN2sqqf93PbGDhpM96QcSTz2H4k3r2PTSEzRJaUHqZdey4an7ArAmvpc2bCBR7Voz/vATSOzbg6NGP8j3w8+rUW7dVxP585o7ARg4djSdLj6Lle+7b0rumDmPSRdc69e4/cnldDLh3Ze57IHniIpL5M17rqFL36NJatmmWrnWh3TbZzJs5g9fk5jWitLiIj9EHBiqpwNr1q0PTZJSWHfvtYS260Tzi65h45N31iiXeOalZP/6HQVzppF80TXEDBhG7pSfyP75G7J//gaAiB6HEzvsFFy7CiuX2zz6fpyFBf5aHb+I6380Ya1a8tepZxHVrSud772TeZfUPIblLVzEzqnT6flO9Rsbmz/8mM0ffgxA/MD+tLzw/IMuqQjw4YTpjPl8Eu8/dkWgQwkop9PJK88/zbMvjyExKZnr/u9ijhpwLG3atqss07vvERw94FiMMaxds5rH7ruLcZ+PZ/3aNfzw3Te8/u4HhASHcPetN3LkMf1p0bLVfr6xYTIGLujdghf/WEtOcTn3DuvE39vy2J5fWlkmLMTBBb1b8Mqfa8kuKieyqTuNsqOglMd+XVn5Oc+efBgLtuYGYjVEAqpRjbG4eNkKWrVIpWVaKiEhIYwYNoTJU6dXK9Oze1eioyIB6H7YoezIyKqcV+F0UlpaSkWFk5KSUpIS4v0av78sXrmGVqnJtExJpklIMCMGHc3vM+dUK9PrsM5ER0YA0KNLR3Zk7ayc17fboZXzGrvFy1fRKi2Vlqkp7m1qyEB+nzazWpleXQ8lOnL3NtWFHZk7a/uoRm3J+q20SoqlRWIsIcEOTuh7GJP/rp5AjI9qRtc2aQQ7qu92EqMjObRVCgDNQpvSNiWBjNzGcxK8dOtOWsZG0iI2khCHg+GHtWbKys3Vyvyxcgsn9WiHMYZuLRIoLC0js6D4gJ/9wi/zuHlYLwzGV+EHVPdThzPrw/EArP9rAWExkUQ1r5lw3p1UBAgJC8Va67cY64OoPv3I/fN3AIrXrMQR3ozgmNga5Zod1p282dMAyJk6ici+/QAITWvFriV/A1C2fQtNEpNxRMX4J3g/azViCGs//xaAzLl/0yQ6irDkmtvU1t+mVr7Omr+Y8NRkv8UYaFvWrCC+eSpxyakEh4TQ7ZghLJ87/cALeuTtzGTV/Fn0HXqSD6MMPNXTgUX0PIK8mVMAKFm3iqDwZjiia+6bwrt0o2DeDADyZkwmomfNXguRRwwgf/afPo23Pkg4diDpE38EIH/xEoIjI2lSy/VI4cpVlGzfvt/PSj5hODt++sUncdZ30+avIidvV6DDCLgVy5aS1qIlqWktCAkJYfCw4cyYOqVambDwcIxxn0eWFBdXvt60YT2HHNaV0NAwHMHBdO/Vm2l/TPb3KvhF27hwMgpLydpVhtNlmbMphx6p1RuFHNEqhgVbc8kuKgegoLSixucckhRJ5q7SyjIiB5P9JhaNMWfs789fQXorIzOL5klJle+TkxLZkZm1z/L/mfAD/Y86orLsZRecw3Gnn8uQU84kIqIZRx95uM9jDoQdO7NpnrjnJKV5QjwZWTn7LP/1T5MZcHhPP0RW/2Rk7aR5UkLl++TEBDKy9p04HP/9L/Q/sk/le4Phqtvv55wrb+LL7370aayBtCO3gOTYPQfg5NgoMnLr3opga1YuKzal061tiwMXbiAyCopJrtKiMDkqvEbSMKOgiOSoPWWSIsPJLHC3YDEGrv/4dy58+0fGz1tdWeaPlVtIjAynU/OaF2mNRUxaMjmbt1W+z92STkxa81rLXvLeczybPofmXdoz+dVxldPbHdWb+xf+yA0/jCPl0I6+DjkgguPiKd+5p8tNeXYWwbHVL0QdEVE4d+0Cl3u8mYrsLEI8ZUo2rSPqcPfQBGHtOhGSkERIXAKNUXhKMru2ple+37UtnfCUpH2WN8HBtD9nJFsnTauclnh4T0b+8R+Gff4WMZ07+DTeQMjPziI6fk+dRMclUrCz5rnU5lXLeO32f/HhE3exY/P6yuk/vP8awy+6GhPUqO5d16B6OrCQmDgqsqvcwM/ZSUhMXLUyjohIXMVV9k05OwmJrV7GNGlCRNdeFMzfc2PXWkvLWx+mzQPPEz1wuA/Xwr+aJiVSmr6j8n3pjgyaJtW9B0dQaFPiju5H5qTGmQgS72RlZpCYtOfGWGJSMlmZNbvoTpvyO5edewb3jbqZ2+97CIA27TuwaOEC8vJyKSkp5q+Z08ncsaPGso1BTFhItWRgbnE5sWEh1cokR4YS3sTBqEEduG9YJ/q1rnn+fXirGOZsyvV1uCL10oG6Qp+yn3kWGP8PxvI/q62Vyu67LnubPW8B4yf8wIdvucfbyssvYPKfM/jp68+IjIxg1H0PM+GnXznlhON8GnMg1NqaZx8Nnv5auITxP//Oxy886tug6qlat6l9VNbs+X8z/vtf+Oi15yqnffT6aJIS4tmZk8uVo+6jbesW9O3RzWfx1id1bUNXVFLGbW99yZ3nHE9EWFOfxBQIltq2ob3K7Ocn+d7lw0mMDCd7VwnXfTyJNglRHJIaz7t/LuH1i4b84/HWJ7Xuv/fRGvHD/7sDExTEea8+Qt9zT2HmuC/ZNH8J97U+htJdRXQdMYhrvxnLg50G+zhq/6t1n7R3Ne3nB5k54UtSLr6a9k++SunmDRRvWAsu5z8aY71Rh20K4KjnHmTHzLlkzJoHwM5Fy/iq51AqdhWRNmwgQz56jfFHNLYhU2rbIVWvt5S2HRk15t80DQtj1fxZfPrsA9z66sesnDeTiOgY0tp3Zv3Shf4JN2BUTwdUy+/twEfEmj/JiB6HU7xmRbVu0JuevpuKvBwckdG0vO1hyrZvoXh1zfFSG5rajnv/TSP8hIEDyFu46KDsBi1VeHlt3H/QEPoPGsKiBfMZN/YNnnv1DVq3act5F13KnTddR1h4OO07dMLhcPgjar+r7RRp75pzGGgdG84LU9bSxGG4a2gn1u0sIqPQ3V3aEWTokRrN+EX7b0ks/uNyHVw9mAJtv4lFa+3l+5u/L8aYq4CrAF5/4RmuuPSi/+Zj6iw5KZH0jD0Dre7IyKy1O/PKNWt56KnRvPHC08REu1tZzZozj7SU5sTFxgAw7NgB/L14SaNMLDZPiCe9Snfd9KydJMXXvOuyct1GHnxpLG89fjcxnu7jB5vkxATSq3SX35GZRWJCXI1yK9eu58HnXubNZx8lJjqqcvru7S8+NoahA45i8fJVjTKxmBwTyY6cvMr3O3LySYzxfpspdzq57a0vOOmIrgzrfYgvQgyY5MhwduTtGT9rR34RCZFh1ctEhbMjf0+ZjIIiEiLdLRgTPf/GNQtlcOeWLNm6k8jQJmzLLeT8t35wl88v4sKxP/LhFSeQEFH9sxuaY6+7mP5Xng/Axjl/E9sytXJeTIvm5G7b991y63Ix9/OJHHfHVcwc92W1LtJLfpzC+WMep1l8LLt27ruFdkMRd9xJxA52J7SK160iJH5Pi5aQuAQqcqu3rHYW5ONo1gyCgsDlIjgugfIcdxlXcTFbx75UWbbTS+9RlplOY9HlXxfQ6eKzAMhasIRmVVq9NkttTlF67QOs97jjOkITYvn9kocqp5UX7Olat/W3qQQ99yBN42Iozc71TfABEBWXSN7OPedSedmZRMZVP5cKDW9W+bpT735MeOclduXnsXHFElbMncGqBX9RUVZGaXERX77yBGff1PjG7FQ91S5m8AhiBrhbEJZsWE1wldbPwbHxVORmVyvvLMwnKKzKvqmWMlGHDyD/r+rdoCvy3PtxZ0EehQv+IqxtxwabWEw75yxSzjgVgIKly2jafE8Ls6bJSZTV0sLsQJKOP+6g7QYteyQkJZOZsee8KTNjB/EJ++6R0L1Xb7Zt3UJebg7RMbGcOPI0Thx5GgDvvPEaiUn7buHfkOUUlxMXvqeFYkxYCLnF5TXKFKYXUOZ0UeaE1ZmFtIwJrUwsdm0eyaacolq7SIscDLzuf2GMOckYc6cx5sHdf/sqa60da63ta63t66+kIkDXQ7qwcfNWtmzbTnl5OT/+9juDBhxdrcz29B3ceveDPPXgPbRp1bJyekrzJBYtXUZxSQnWWv6aO5+2bVr7LXZ/6tq5PRu3prMlPYOy8gp+nDKDwf36ViuzLSOLmx59nqfvuJ42LVL38UmNX9cundi0ZRtbtqe7t6nfpzL4mH7VymzfkcEtDzzOU/fdTpuWe7rwFhWXsKuoqPL1jDkL6Ni2cW5Th7VJY2NGNluyciivcPLT3KUM6tHJq2WttTz04QTaNk/kkuOO8nGk/ndoWjybswvYmlNIudPJL0s3cmyn6l29B3Zqwfd/r8Nay+ItWUQ0bUJiZBjFZRXsKnWf2BSXVTBr3XY6JMXQMTmW324/i4k3n8bEm08jKSqcT64a0eCTigB/jPmIJ3qdyBO9TmThN7/Q7xL3qBttj+xFSV4B+bUkgRLb7/lddT9lKDtWuJ+IHFVl7Lw2h/fABJlGkVQEyP71e9beeyNr772R/LmziBngbr0a1qEzzuJdVOTWXM9dyxYTfYT7ae2xA4dSMO8vAILCm2Ec7vuMsYOPZ9eKJbiKDzzGZ0Ox4t1P+W7QGXw36Aw2/TCJ9ue6L+AT+/agLL+A4h01t6mOF51F2pD+/HHl7dVafIRVGRojoXc3CDKNKqkIkNahCzu3byVnx3YqystZPP13uvStfi5VkJNd2aJ/y+rlWJclPDKK4RdeyR1vfcmoMf/mnFsfpG3XXo0iWVYb1VPtcif/yIZHb2XDo7dSsOAvoo8aBEBou064infhzKu5bypauZjIPu66iz56MIULZ1fOCwoLJ7zzYRQs/KtymmnSlKCmoZWvww/tSenWTT5cK9/a+sVXzD3vYuaedzFZk6fS/OQRAER160pFYSFl+xmCpzaOiGbE9OlF1pSpBy4sjVqXQw5l6+bNbN+2lfLycib/9gtHDzi2WpmtmzdX7qdWrVxOeXk5UdExAORku5P8O9K3M23K7ww5rrG10HfbkF1EUkRT4ps1wRFkOLxVLH9vq97ad+HWPDokNCPIQBOHoW18eLWHuxzRKpbZ6gYtBzGvngptjHkTCAcGA+8AZwGz97tQAAQHO7h31E1cc8udOF0uTj95BB3ateWL8d8BcM4ZI3nzvQ/Jzc/n8dEvAeBwOPj8/bfoftihHDf4WM659CqCgx106dSRs089OYBr4zvBDgf3Xf9/XHnvk7hcLk4fPoiObVry74m/AnDeycfxxidfkVdQyKOvvVu5zJevuZ9qePtTLzN70TJy8woYfOG13HDx2Zx5QuPskhkc7ODeW67l6tvvd29TJw6nQ9vWfP7t9wCce+pJvPHBp+TlFfD4i+4n8zkcQXwx9hV25uRw8/2PA+6nsp04bBD9j+y7z+9qyIIdQdx73giuffkTnC7Lacf0pENqEl/8MReAc47tS1ZeIec9+Ta7SkoJMoaPJ/3FNw9fx6qtO5g4axEd05I4+7G3ALjptCEM6NY4xsMLDgrizhF9ueGT33Fay6k929M+KYav5q4C4Ky+nejfMZXpa7Zy6mvfERri4OGR7gTrzl3F3P6F+8LA6bKc0LUNR3c4eBL9S36YTNcTB/PYmj8oKyrmg8vvqJx3w/fv89EVd5GfnsllHzxPaFQEGMPWv5fz6bX3A9D7rBEMvPYiXBVOyopLeOe8GwO1Kj5VuHAOkT370umFd3CVlbLlrRcr57W+42G2vv0KFbnZpH/2Pi1vvJOksy+mZOM6cqb8DEDT1Ja0uPY2cLko2bqZrWNfDtSq+NyWX/8g7biBnDH3Z5zFJUy78d7KecP+/RbTb7mf4vRMjnr+IQo3b+Oknz4DYOPE3/h79BhajxxO58vPx1ZU4Cwp5Y8rRgVqVXzG4XBw8r9u4oMn7sTlctF78AiSW7Zl9i/uc6kjho9k6aw/mP3LtwQ5HIQ0aco5tz6wz6FnGivV04HtWjyPiG59aPfkm7jKSkl//5XKeS1ufoD0ca9RkZdD5lcfknr1KBJPv5CSTevIm/ZrZbnIXv3YtXQhtmzPBXxwVAxp198NgAlykD97KruWLvDfivnQzmnTiet/NP2++xpnSQkrHn6scl73V19kxaNPUJaZRdr559Dq0otpEh/H4V98ws5pM1j56JMAJA4eRPas2bhKSgK0FoH30VNXM7BPZxJiIlj302geffNbxn3T+B/+szdHcDA3jrqTu265AZfLyYiTT6VNu/ZMGP8VAKeccRZTp0zi1x+/Jzg4mCZNm/LA409V7qcevvcO8vPyCA4O5qbb7yYyKmp/X9dguSx8Nn8LtwxsR5AxTF+fzfb8Ega2d7dCn7p2J+kFpSxNz+fB4V2wWKaty2Zbvvs31sRhOCQ5ko/nbd7f14g0asabp2caYxZZa7tX+TcCGG+tPeBoyWXZ29S53QuO/IwDFxIAXKEHZ7fsunKtmBXoEBqE8q1rAx1Cg3H7Re8FOoQG4YYLDgt0CA3G3J/WBTqEBiF88qRAhyCNTPdXrgt0CA3C9nkaL81bx6NjnzfWTXrlwIWER39bE+gQGoyx5/Q8eO5Y1cFfxw1ulHmoI3+dXC//v73tCr27T1SRMSYVKAfa+iYkERERERERERERqe+86goNTDTGxADPAfNxPyjpHV8FJSIiIiIiIiIiIvWbV4lFa+3uAT6+NsZMBEKttXn7W0ZEREREREREREQaL28f3nJJLdOw1n74z4ckIiIiIiIiIiJSdy6nK9AhHFS87Qp9eJXXocBQ3F2ilVgUERERERERERE5CHnbFfrGqu+NMdHARz6JSEREREREREREROo9b58KvbcioOM/GYiIiIiIiIiIiIg0HN6OsTgB95OgwZ2MPBT4wldBiYiIiIiIiIiI1JV12gMXkn+Mt2Msjq7yugLYaK3d4oN4REREREREREREpAHwNrE4Fyi21rqMMZ2A3saYHdbach/GJiIiIiIiIiIiIvWUt2MsTgVCjTFpwCTgcmCcr4ISERERERERERGR+s3bxKKx1hYBZwCvWmtPxz3OooiIiIiIiIiIiByEvO0KbYwxRwEXAv+q47IiIiIiIiIiIiI+p4e3+Je3LRZvBu4B/mOtXWqMaQdM9l1YIiIiIiIiIiIiUp951erQWjsV9ziLu9+vA27yVVAiIiIiIiIiIiJSv3mVWDTGJAJ3AocBobunW2uH+CguERERERERERERqce8HSfxE+Bz4GTgGuBSINNXQYmIiIiIiIiIiNSVy+kKdAgHFW/HWIy31r4LlFtr/7DW/h/Qz4dxiYiIiIiIiIiISD3mbYvFcs+/240xJwHbgBa+CUlERERERERERETqO28Ti48bY6KBUcCrQBRwi6+CEhERERERERERkfrN28Ti2cA0a+0SYLAxJg4YDUzwWWQiIiIiIiIiIiJ1YF020CEcVLwdY7G7tTZ39xtrbTbQyycRiYiIiIiIiIiISL3nbWIxyBgTu/uNp8Wit60dRUREREREREREpJHxNjn4PDDDGPMVYIFzgCd8FpWIiIiIiIiIiIjUa14lFq21Hxpj5gJDAAOcYa1d5tPIRERERERERERE6sDl1BiL/uR1d2ZPIlHJRBEREREREREREfF6jEURERERERERERGRSkosioiIiIiIiIiISJ0psSgiIiIiIiIiIiJ15vUYiyIiIiIiIiIiIvWZdboCHcJBRS0WRUREREREREREpM583mLxtslZvv6KRmHUse0DHUKD8d3KzECH0CBc2+v4QIfQIKx+6YJAh9BgjFn7VaBDaBCyxn8U6BAajLIf1gY6hAYh0gQ6gobBaQMdQcNRUVwe6BAahIL0XYEOocFYt/SVQIfQILQbelOgQ2gQmiW2DHQIDcbYc3oGOgQRtVgUERERERERERGRutMYiyIiIiIiIiIi0ihYdWPwK7VYFBERERERERERkTpTYlFERERERERERETqTIlFERERERERERERqTONsSgiIiIiIiIiIo2CS2Ms+pVaLIqIiIiIiIiIiEidKbEoIiIiIiIiIiIidabEooiIiIiIiIiIiNSZxlgUEREREREREZFGwbpcgQ7hoKIWiyIiIiIiIiIiIlJnSiyKiIiIiIiIiIhInSmxKCIiIiIiIiIiInWmxKKIiIiIiIiIiIjUmR7eIiIiIiIiIiIijYLLaQMdwkFFLRZFRERERERERESkzpRYFBERERERERERkTpTYlFERERERERERETqTGMsioiIiIiIiIhIo2A1xqJfqcWiiIiIiIiIiIiI1JkSiyIiIiIiIiIiIo2UMSbOGPOrMWa159/YWsq0NMZMNsYsN8YsNcbc7M1nK7EoIiIiIiIiIiLSeN0NTLLWdgQmed7vrQIYZa09BOgHXG+MOfRAH6wxFkVEREREREREpFGwTlegQ6iPTgUGeV5/AEwB7qpawFq7HdjueV1gjFkOpAHL9vfBarEoIiIiIiIiIiJSjxljrjLGzK3yd1UdFk/2JA53JxCTDvBdbYBewF8H+mC1WBQREREREREREanHrLVjgbH7mm+M+Q1oXsus++ryPcaYCOBr4BZrbf6ByiuxKCIiIiIiIiIi0oBZa4fta54xZocxJsVau90YkwJk7KNcCO6k4ifW2vHefK8SiyIiIiIiIiIi0ii4nDbQIdRH3wGXAk97/v127wLGGAO8Cyy31r7g7QdrjEURERER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br/7oHwo5W4fPXU1A/t0JiEmgnU/jebRN79l3Dd/HnjBRsTpcnHnaz/w9ZMX4wgK4pOfF7BiYyaXn9QXgPe/n0vnVgm8cecZOF0uVm7M5MYX3A+CS4yJ4OOHzgPA4Qji68mLmTR3TcDWRfZw1uPxCBsjU9vYaDUKGdMGeBk4BndicTpwi7V2w4GW9WdX6IZs1LFtAx1Cg/Hdyn0/FET2uLbrwXuSVBdLL70g0CE0GD1eeDLQITQIWeM/CnQIDcY3T/wc6BAahPipvwc6hAbBqTNOrx36zJWBDqFBWPd7423190/rvXRWoENoENoNvSnQITQIzRIbf4+vf0rOL48cPHes6uA/zQ9rlGcFp6cvrZf/3161WPQkEBvfow9FRERERERERETkv+LVGIvGmE7GmEnGmCWe992NMff7NjQRERERERERERGpr7x9eMvbwD1AOYC1dhFwnq+CEhERERERERERqSunbZx/9ZW3icVwa+3svaZV/NPBiIiIiIiIiIiISMPgbWIxyxjTHveDWzDGnAVs91lUIiIiIiIiIiIiUq959fAW4HpgLNDFGLMVWA9c6LOoREREREREREREpF47YGLRGOMArrXWDjPGNAOCrLUFvg9NRERERERERERE6qsDJhattU5jTB/P612+D0lERERERERERKTunLYeP+mkEfK2K/QCY8x3wJdAZXLRWjveJ1GJiIiIiIiIiIhIveZtYjEO2AkMqTLNAkosioiIiIiIiIiIHIS8Sixaay/3dSAiIiIiIiIiIiLScHiVWDTGdALeAJKttV2NMd2Bkdbax30anYiIiIiIiIiIiJecGmLRr4K8LPc2cA9QDmCtXQSc56ugREREREREREREpH7zNrEYbq2dvde0in86GBEREREREREREWkYvE0sZhlj2uN+YAvGmLOA7T6LSkREREREREREROo1b58KfT0wFuhijNkKrAcu9FlUIiIiIiIiIiIideS0GmTRn7xNLFpr7TBjTDMgyFpbYIxp68vAREREREREREREpP7ytiv01wDW2l3W2gLPtK98E5KIiIiIiIiIiIjUd/ttsWiM6QIcBkQbY86oMisKCPVlYCIiIiIiIiIiIlJ/HagrdGfgZCAGOKXK9ALgSh/FJCIiIiIiIiIiUmdODbHoV/tNLFprvwW+NcYcZa2d6aeYREREREREREREpJ7zdozF040xUcaYEGPMJGNMljHmIp9GJiIiIiIiIiIiIvWWt4nF4dbafNzdorcAnYA7fBaViIiIiIiIiIiI1GveJhZDPP+eCHxmrc32UTwiIiIiIiIiIiLSABzo4S27TTDGrACKgeuMMYlAie/CEhERERERERERqRun1dNb/MmrFovW2ruBo4C+1tpyYBdwqi8DExERERERERERkfprvy0WjTFDrLW/G2POqDKtapHxvgpMRERERERERERE6q8DdYU+FvgdOKWWeRYlFkVERERERERERA5K+00sWmsf8vx7uX/CERERERERERER+e84NcSiX3n18BZjzG21TM4D5llrF/6jEYmIiIiIiIiIiEi959XDW4C+wDVAmufvKmAQ8LYx5k7fhCYiIiIiIiIiIiL1lVctFoF4oLe1thDAGPMQ8BUwEJgHPLvPBSOa/K8xHhSCqj8UR/Yjsqm3m+1BzlUR6AgahPCkiECH0GAUTP4m0CE0CKW5hYEOocHILHUGOoQGYeu2/ECH0CA4Xer35K3O5a5Ah9AgFFaonrz16G9rAh1Cg9AssWWgQ2gQdmVuDnQIIlIH3mZoWgFlVd6XA62ttcXGmNJ/PiwREREREREREZG60RiL/uVtYvFTYJYx5lvP+1OAz4wxzYBlPolMRERERERERERE6i2vEovW2seMMT8A/QEDXGOtneuZfaGvghMREREREREREZH6yduHtwCEAfnW2peAjcaYtr4JSUREREREREREROo7r1oseh7W0hfoDLwPhAAfA8f4LjQRERERERERERHvOa0GWfQnb1ssng6MBHYBWGu3AZG+CkpERERERERERETqN28Ti2XWWgtYAM9DW0REREREREREROQgdcDEojHGABONMW8BMcaYK4HfgLd9HZyIiIiIiIiIiIjUTwccY9Faa40xpwF3Afm4x1l80Fr7q49jExERERERERERkXrKq4e3ADOBXGvtHb4MRkRERERERERE5L/l1LNb/MrbxOJg4GpjzEY8D3ABsNZ290lUIiIiIiIiIiIiUq95m1gc4dMoREREREREREREpEHxKrFord3o60BERERERERERESk4fC2xaKIiIiIiIiIiEi95rQaZNGfggIdgIiIiIiIiIiIiDQ8SiyKiIiIiIiIiIhInSmxKCIiIiIiIiIiInWmMRZFRERERERERKRRcGqIRb9Si0URERERERERERGpMyUWRUREREREREREpM6UWBQREREREREREZE60xiLIiIiIiIiIiLSKDitBln0J7VYFBERERERERERkTpTYlFERERERERERETqTIlFERERERERERERqTMlFkVERERERERERKTO9PAWERERERERERFpFJx6dotfqcWiiIiIiIiIiIiI1JkSiyIiIiIiIiIiIlJnSiyKiIiIiIiIiIhInWmMRRERERERERERaRScVoMs+lOjSyxuXzaf+ePfxrpctDvqOA497qwaZXasXsyC8e/iclbQtFkUQ29+Emd5GZNevhdXRTkul5OWPY+m24kXBGAN/G/OrBm88dJoXE4nJ5xyGuddcnmt5VYuW8rNV13GvY8+xcAhw/wcZeCs+3sOkz4ag8vlosegEfQbeV61+ZuW/c3XLzxITGJzADod3p9jzriYnds2892rj1eWy81Ip/9Zl3L4iDP8Gr+/TJv5F8+8+CpOl4szRp7EFZdcWG3+xJ9+5b2PPgUgPDyMB+68jc4dOwDw4WdfMP677zHG0LF9Wx67/26aNm3q93XwpaRz/0Wzbr2xZaVsH/capZvW1SgTEp9EylW34QiPoGTTera/9zI4KwAI63QYSef+H8bhwFlYwObRDwAQM+QkYgYcBwby/vyNnEkT/bpe/7SZ69J5ftICXNZyavd2XNqvS7X51lqen7SQGeu2ExoSzIMjDqdL81hKK5xc/elkypwunC7L0M4tuKr/YZXLfT5vNV/OX4MjKIhj2qdw06Du/l61f1zMSecT2qkbtryM7K/fo3z7phplHLEJxJ9zNUFhzSjfvpGdX70DTifhPY4kcsAIAGxZKTnffUR5+hYAIo4+jog+AwAo27GF7PHvQUWF/1bMj4aPfoD2xx9LeVExE6++i/SFy2qUOemNJ0np1RWMIXvNBiZcdRflu4oCEK3/6FzKO+nL57Ng/DtY66Jdv+PoMuzMGmUyVi9m4X/exbqcNGkWxeAbn8BZXsbkV+/DVVGOdTlp0eNoDhtxfgDWwPdSL7+GqF6H4yotZfOY5ylev7ZGmSaJybS65W6CIyIpXr+GTa+OxjorcDSLoOW1t9IkOQVbXsbmN16kZPNGABJGnErc0BMwxrBz0k9k/fCNn9fMd/o8eS+pwwZSUVTMrJvuJWfR8hpljn7jWeJ6HoarvIKdCxYze9TD2Cr76bie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\n",
      "text/plain": [
       "<Figure size 1800x1440 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = (25,20))\n",
    "sns.heatmap(bike.corr(), annot = True, cmap=\"RdBu\")\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 28,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1800x1440 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Let's check the correlation coefficients to see which variables are highly correlated. Note:\n",
    "# here we are considering only those variables (dataframe: bike_new) that were chosen for analysis\n",
    "\n",
    "plt.figure(figsize = (25,20))\n",
    "sns.heatmap(bike_new.corr(), annot = True, cmap=\"RdBu\")\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Insights:<br>\n",
    "\n",
    "The heatmap clearly shows which all variable are multicollinear in nature, and which variable have high collinearity with the target variable.<br>\n",
    "\n",
    "We will refer this map back-and-forth while building the linear model so as to validate different correlated values along with VIF & p-value, for identifying the correct variable to select/eliminate from the model.<br>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > Creating Dummy Variables"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "We will create DUMMY variables for 4 categorical variables 'mnth', 'weekday', 'season' & 'weathersit'."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This below code does 3 things:<br>\n",
    "        1) Create Dummy variable. <br>\n",
    "        2) Drop original variable for which the dummy was created. <br>\n",
    "        3) Drop first dummy variable for each set of dummies created."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 29,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 730 entries, 0 to 729\n",
      "Data columns (total 30 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   yr                          730 non-null    int64  \n",
      " 1   holiday                     730 non-null    int64  \n",
      " 2   workingday                  730 non-null    int64  \n",
      " 3   temp                        730 non-null    float64\n",
      " 4   atemp                       730 non-null    float64\n",
      " 5   hum                         730 non-null    float64\n",
      " 6   windspeed                   730 non-null    float64\n",
      " 7   cnt                         730 non-null    int64  \n",
      " 8   season_Spring               730 non-null    uint8  \n",
      " 9   season_Summer               730 non-null    uint8  \n",
      " 10  season_Winter               730 non-null    uint8  \n",
      " 11  mnth_Aug                    730 non-null    uint8  \n",
      " 12  mnth_Dec                    730 non-null    uint8  \n",
      " 13  mnth_Feb                    730 non-null    uint8  \n",
      " 14  mnth_Jan                    730 non-null    uint8  \n",
      " 15  mnth_Jul                    730 non-null    uint8  \n",
      " 16  mnth_Jun                    730 non-null    uint8  \n",
      " 17  mnth_Mar                    730 non-null    uint8  \n",
      " 18  mnth_May                    730 non-null    uint8  \n",
      " 19  mnth_Nov                    730 non-null    uint8  \n",
      " 20  mnth_Oct                    730 non-null    uint8  \n",
      " 21  mnth_Sep                    730 non-null    uint8  \n",
      " 22  weekday_1                   730 non-null    uint8  \n",
      " 23  weekday_2                   730 non-null    uint8  \n",
      " 24  weekday_3                   730 non-null    uint8  \n",
      " 25  weekday_4                   730 non-null    uint8  \n",
      " 26  weekday_5                   730 non-null    uint8  \n",
      " 27  weekday_6                   730 non-null    uint8  \n",
      " 28  weathersit_Light_Snow_Rain  730 non-null    uint8  \n",
      " 29  weathersit_Mist_Cloudy      730 non-null    uint8  \n",
      "dtypes: float64(4), int64(4), uint8(22)\n",
      "memory usage: 61.4 KB\n"
     ]
    }
   ],
   "source": [
    "bike_new = pd.get_dummies(bike_new, drop_first=True)\n",
    "bike_new.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 30,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(730, 30)"
      ]
     },
     "execution_count": 30,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike_new.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 31,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['yr', 'holiday', 'workingday', 'temp', 'atemp', 'hum', 'windspeed',\n",
       "       'cnt', 'season_Spring', 'season_Summer', 'season_Winter', 'mnth_Aug',\n",
       "       'mnth_Dec', 'mnth_Feb', 'mnth_Jan', 'mnth_Jul', 'mnth_Jun', 'mnth_Mar',\n",
       "       'mnth_May', 'mnth_Nov', 'mnth_Oct', 'mnth_Sep', 'weekday_1',\n",
       "       'weekday_2', 'weekday_3', 'weekday_4', 'weekday_5', 'weekday_6',\n",
       "       'weathersit_Light_Snow_Rain', 'weathersit_Mist_Cloudy'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 31,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike_new.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' >  SPLITTING THE DATA\n",
    "Splitting the data to <span style='background: lightGreen '> Train and Test</span>  : - We will now split the data into TRAIN and TEST (70:30 ratio)\n",
    "We will use train_test_split method from sklearn package for this"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* Check the shape & info  before spliting"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 32,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(730, 30)"
      ]
     },
     "execution_count": 32,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "bike_new.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 33,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "RangeIndex: 730 entries, 0 to 729\n",
      "Data columns (total 30 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   yr                          730 non-null    int64  \n",
      " 1   holiday                     730 non-null    int64  \n",
      " 2   workingday                  730 non-null    int64  \n",
      " 3   temp                        730 non-null    float64\n",
      " 4   atemp                       730 non-null    float64\n",
      " 5   hum                         730 non-null    float64\n",
      " 6   windspeed                   730 non-null    float64\n",
      " 7   cnt                         730 non-null    int64  \n",
      " 8   season_Spring               730 non-null    uint8  \n",
      " 9   season_Summer               730 non-null    uint8  \n",
      " 10  season_Winter               730 non-null    uint8  \n",
      " 11  mnth_Aug                    730 non-null    uint8  \n",
      " 12  mnth_Dec                    730 non-null    uint8  \n",
      " 13  mnth_Feb                    730 non-null    uint8  \n",
      " 14  mnth_Jan                    730 non-null    uint8  \n",
      " 15  mnth_Jul                    730 non-null    uint8  \n",
      " 16  mnth_Jun                    730 non-null    uint8  \n",
      " 17  mnth_Mar                    730 non-null    uint8  \n",
      " 18  mnth_May                    730 non-null    uint8  \n",
      " 19  mnth_Nov                    730 non-null    uint8  \n",
      " 20  mnth_Oct                    730 non-null    uint8  \n",
      " 21  mnth_Sep                    730 non-null    uint8  \n",
      " 22  weekday_1                   730 non-null    uint8  \n",
      " 23  weekday_2                   730 non-null    uint8  \n",
      " 24  weekday_3                   730 non-null    uint8  \n",
      " 25  weekday_4                   730 non-null    uint8  \n",
      " 26  weekday_5                   730 non-null    uint8  \n",
      " 27  weekday_6                   730 non-null    uint8  \n",
      " 28  weathersit_Light_Snow_Rain  730 non-null    uint8  \n",
      " 29  weathersit_Mist_Cloudy      730 non-null    uint8  \n",
      "dtypes: float64(4), int64(4), uint8(22)\n",
      "memory usage: 61.4 KB\n"
     ]
    }
   ],
   "source": [
    "bike_new.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 34,
   "metadata": {},
   "outputs": [],
   "source": [
    "import sklearn\n",
    "from sklearn.model_selection import train_test_split"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* We should specify 'random_state' so that the train and test data set always have the same rows, respectively"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 35,
   "metadata": {},
   "outputs": [],
   "source": [
    "df_train, df_test = train_test_split(bike_new, train_size = 0.70, test_size = 0.30, random_state = 333)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* Verify the info and shape of the dataframes after split:"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 36,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Int64Index: 510 entries, 483 to 366\n",
      "Data columns (total 30 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   yr                          510 non-null    int64  \n",
      " 1   holiday                     510 non-null    int64  \n",
      " 2   workingday                  510 non-null    int64  \n",
      " 3   temp                        510 non-null    float64\n",
      " 4   atemp                       510 non-null    float64\n",
      " 5   hum                         510 non-null    float64\n",
      " 6   windspeed                   510 non-null    float64\n",
      " 7   cnt                         510 non-null    int64  \n",
      " 8   season_Spring               510 non-null    uint8  \n",
      " 9   season_Summer               510 non-null    uint8  \n",
      " 10  season_Winter               510 non-null    uint8  \n",
      " 11  mnth_Aug                    510 non-null    uint8  \n",
      " 12  mnth_Dec                    510 non-null    uint8  \n",
      " 13  mnth_Feb                    510 non-null    uint8  \n",
      " 14  mnth_Jan                    510 non-null    uint8  \n",
      " 15  mnth_Jul                    510 non-null    uint8  \n",
      " 16  mnth_Jun                    510 non-null    uint8  \n",
      " 17  mnth_Mar                    510 non-null    uint8  \n",
      " 18  mnth_May                    510 non-null    uint8  \n",
      " 19  mnth_Nov                    510 non-null    uint8  \n",
      " 20  mnth_Oct                    510 non-null    uint8  \n",
      " 21  mnth_Sep                    510 non-null    uint8  \n",
      " 22  weekday_1                   510 non-null    uint8  \n",
      " 23  weekday_2                   510 non-null    uint8  \n",
      " 24  weekday_3                   510 non-null    uint8  \n",
      " 25  weekday_4                   510 non-null    uint8  \n",
      " 26  weekday_5                   510 non-null    uint8  \n",
      " 27  weekday_6                   510 non-null    uint8  \n",
      " 28  weathersit_Light_Snow_Rain  510 non-null    uint8  \n",
      " 29  weathersit_Mist_Cloudy      510 non-null    uint8  \n",
      "dtypes: float64(4), int64(4), uint8(22)\n",
      "memory usage: 46.8 KB\n"
     ]
    }
   ],
   "source": [
    "df_train.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 37,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(510, 30)"
      ]
     },
     "execution_count": 37,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_train.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 38,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Int64Index: 219 entries, 22 to 313\n",
      "Data columns (total 30 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   yr                          219 non-null    int64  \n",
      " 1   holiday                     219 non-null    int64  \n",
      " 2   workingday                  219 non-null    int64  \n",
      " 3   temp                        219 non-null    float64\n",
      " 4   atemp                       219 non-null    float64\n",
      " 5   hum                         219 non-null    float64\n",
      " 6   windspeed                   219 non-null    float64\n",
      " 7   cnt                         219 non-null    int64  \n",
      " 8   season_Spring               219 non-null    uint8  \n",
      " 9   season_Summer               219 non-null    uint8  \n",
      " 10  season_Winter               219 non-null    uint8  \n",
      " 11  mnth_Aug                    219 non-null    uint8  \n",
      " 12  mnth_Dec                    219 non-null    uint8  \n",
      " 13  mnth_Feb                    219 non-null    uint8  \n",
      " 14  mnth_Jan                    219 non-null    uint8  \n",
      " 15  mnth_Jul                    219 non-null    uint8  \n",
      " 16  mnth_Jun                    219 non-null    uint8  \n",
      " 17  mnth_Mar                    219 non-null    uint8  \n",
      " 18  mnth_May                    219 non-null    uint8  \n",
      " 19  mnth_Nov                    219 non-null    uint8  \n",
      " 20  mnth_Oct                    219 non-null    uint8  \n",
      " 21  mnth_Sep                    219 non-null    uint8  \n",
      " 22  weekday_1                   219 non-null    uint8  \n",
      " 23  weekday_2                   219 non-null    uint8  \n",
      " 24  weekday_3                   219 non-null    uint8  \n",
      " 25  weekday_4                   219 non-null    uint8  \n",
      " 26  weekday_5                   219 non-null    uint8  \n",
      " 27  weekday_6                   219 non-null    uint8  \n",
      " 28  weathersit_Light_Snow_Rain  219 non-null    uint8  \n",
      " 29  weathersit_Mist_Cloudy      219 non-null    uint8  \n",
      "dtypes: float64(4), int64(4), uint8(22)\n",
      "memory usage: 20.1 KB\n"
     ]
    }
   ],
   "source": [
    "df_test.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 39,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(219, 30)"
      ]
     },
     "execution_count": 39,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_test.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 40,
   "metadata": {
    "scrolled": false
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Int64Index: 510 entries, 483 to 366\n",
      "Data columns (total 30 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   yr                          510 non-null    int64  \n",
      " 1   holiday                     510 non-null    int64  \n",
      " 2   workingday                  510 non-null    int64  \n",
      " 3   temp                        510 non-null    float64\n",
      " 4   atemp                       510 non-null    float64\n",
      " 5   hum                         510 non-null    float64\n",
      " 6   windspeed                   510 non-null    float64\n",
      " 7   cnt                         510 non-null    int64  \n",
      " 8   season_Spring               510 non-null    uint8  \n",
      " 9   season_Summer               510 non-null    uint8  \n",
      " 10  season_Winter               510 non-null    uint8  \n",
      " 11  mnth_Aug                    510 non-null    uint8  \n",
      " 12  mnth_Dec                    510 non-null    uint8  \n",
      " 13  mnth_Feb                    510 non-null    uint8  \n",
      " 14  mnth_Jan                    510 non-null    uint8  \n",
      " 15  mnth_Jul                    510 non-null    uint8  \n",
      " 16  mnth_Jun                    510 non-null    uint8  \n",
      " 17  mnth_Mar                    510 non-null    uint8  \n",
      " 18  mnth_May                    510 non-null    uint8  \n",
      " 19  mnth_Nov                    510 non-null    uint8  \n",
      " 20  mnth_Oct                    510 non-null    uint8  \n",
      " 21  mnth_Sep                    510 non-null    uint8  \n",
      " 22  weekday_1                   510 non-null    uint8  \n",
      " 23  weekday_2                   510 non-null    uint8  \n",
      " 24  weekday_3                   510 non-null    uint8  \n",
      " 25  weekday_4                   510 non-null    uint8  \n",
      " 26  weekday_5                   510 non-null    uint8  \n",
      " 27  weekday_6                   510 non-null    uint8  \n",
      " 28  weathersit_Light_Snow_Rain  510 non-null    uint8  \n",
      " 29  weathersit_Mist_Cloudy      510 non-null    uint8  \n",
      "dtypes: float64(4), int64(4), uint8(22)\n",
      "memory usage: 46.8 KB\n"
     ]
    }
   ],
   "source": [
    "df_train.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 41,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['yr', 'holiday', 'workingday', 'temp', 'atemp', 'hum', 'windspeed',\n",
       "       'cnt', 'season_Spring', 'season_Summer', 'season_Winter', 'mnth_Aug',\n",
       "       'mnth_Dec', 'mnth_Feb', 'mnth_Jan', 'mnth_Jul', 'mnth_Jun', 'mnth_Mar',\n",
       "       'mnth_May', 'mnth_Nov', 'mnth_Oct', 'mnth_Sep', 'weekday_1',\n",
       "       'weekday_2', 'weekday_3', 'weekday_4', 'weekday_5', 'weekday_6',\n",
       "       'weathersit_Light_Snow_Rain', 'weathersit_Mist_Cloudy'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 41,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_train.columns"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* By Observing tha data we can say that 'temp', 'atemp', 'hum', 'windspeed','cnt' are the numerical variables "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '> Let's make a pairplot of all the numeric variables.</span> <br>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 42,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 900x900 with 30 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Create a new dataframe of only numeric variables:\n",
    "\n",
    "bike_num=df_train[[ 'temp', 'atemp', 'hum', 'windspeed','cnt']]\n",
    "\n",
    "sns.pairplot(bike_num, diag_kind='kde')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The above Pair-Plot  tells us that there is a LINEAR RELATION between 'temp','atemp' and 'cnt'"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 43,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "image/png": "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EREfUCuCiIiIiEQF3RVBRERERKSUUMVWRERExAd8ULBVYisiIiLiB7HqsRURERGRaOCHi8fUYysiIiIiUUEVWxEREREfiPVBOVOJrYiIiIgPqBVBRERERKSUUMVWRERExAd0VwQRERERiQpqRRARERERKSVUsRURERHxAd0VwYd2PdM10iFIEVnZ8pEOQYqo0qAxkQ5Biqh/xv5IhyBFdP0zgyMdghyNs7+OdARqRRARERERKS1UsRURERHxAR8UbJXYioiIiPhBDNGf2aoVQURERESigiq2IiIiIj6gVgQRERERiQoxPkhs1YogIiIiIlFBFVsRERERH1ArgoiIiIhEBd0VQURERESklFDFVkRERMQH1IogIiIiIlFBd0UQERERESklVLEVERER8QEfFGyV2IqIiIj4QYwPmmzViiAiIiIiUUEVWxEREREf8EHBVomtiIiIiB/44WN6PxyjiIiIiPiAKrYiIiIiPmA+6EVQYisiIiLiA/qCBhERERGRUkKJrYiIiIgPmIXvUbT9WU8zW2pmy83s9iOMO83MAmZ28bEeo1oRRERERHygOKuZZhYL/BvoDqQBM81sjHNuUSHjHgcmhGO/qtiKiIiISLi1B5Y751Y65w4A7wN9Chn3f8AnwJZw7FSJrYiIiIgPmFk4HwPMbFaex4ACu0sF1uWZT/OW5Y0nFbgQeDlcx6hWBBEREREfCOddEZxzI4ARRxhS2N5cgflngducc4Fw3YpMia2IiIiIhFsaUDfPfB1gQ4Ex7YD3vaS2OtDbzHKcc6P+150qsRURERHxgWK+je1MoImZNQTWA5cDV+Yd4JxrmBub2RvAF8eS1IISWxERERFfKM4vaHDO5ZjZUEJ3O4gFXnPOLTSzQd76sPXV5qXEVkRERETCzjk3FhhbYFmhCa1zrm849qnEVkRERMQHwnWBVkmmxFZERETEB4qzFSFSlNiWctMWLOPx98YRCDouOutUrut9Vr71Kzdu5Z7XR7F47UaGXdiNvj06ArBqUzq3vPJR7ri0rRkM6XM213Q/o1jj95Np85by2NtjCAQdf+5yGtf96ex861du2MI9r37EojXrGXZxD/r17pK77q0J0/hk0gwcjou7tOeanmcV3LwUoxH39aN351Zs3b6LNpfcG+lwBLj0ufto0ftsDuzdx8i+N7Pul4WHjen/9rPUa3cKgewcVs+YyzsD7ySYk0P5hMr0f/ufVK2XSkxcLF8/9So/vvFRIXuRcKg7YDAJbdsTzMpi9XNPsm/F8sPGlK2VzAm33Els5QT2rljG6mcex+XkABDfoiV1rx+MxcWSs2sXv97xj+I+BCnBSvwXNJhZAzNbEOk4SqJAMMjD73zJizdezejhQxg3Yz4rNuT/4o4qlSpwxxW96XvemfmWN0yuzsf3/Y2P7/sbH9wzkPJly9Dt1ObFGb6vBIJBHnpzFC/d3J8xj93E2OlzWbF+c74xVeIrcvs159O3V+d8y5elbeKTSTN47/6hfPLQjUyes4Q1m9KLM3wp4M3Pv+ePQ56JdBjiadGrKzWbNOTeJl15Z8CdXPnSw4WOm/HOKO5v1o3hp/SgbIXydLrucgC6DrmGjYuW81DrXjzT9XIufvouYsuUKc5D8I2Etu0pl5LKwoF9WfvvZ6n/t2GFjkvtex2bR3/KwoF9CezeTbXuPQGIrVSJen8bxvKH7mHRkOtZ+djw4gy/1LMwPkqqEp/Yym+bv2o99WpWpW6NqpSJi6NX+xZ8N2dJvjHVEuJp0TCVuNjY39zOT4tXUrdGEinVEo9zxP41f8U66tWsRt2a1ULnqkMrvv0539dlUy0hnlNOqHvYuVq5YQstG9ejQrmyxMXG0q5ZQybO1nu9SJr2869k7NwT6TDE07LPeUx/81MAVv30CxUSK5OQXOOwcQvGTcqdXj1jLkl1kgFwDspXrgRAufiK7Nm+g6BXHZTwSuxwBtu+/QaAPUsXE1spnrikqoeNS2jZmozvpwCwbeJXJHYIfdpYtcs57PhxGtlbtwKQs3NH8QQeJWLMwvYoqUpLYhtrZq+a2UIz+8rMKpjZJDNrB2Bm1c1stTfd18xGmdnnZrbKzIaa2U1m9ouZTTezw19BpdSWjF0kJ1XJna+VVIXNGZlHvZ1xMxbQ6/RTwhmaFLAlYyfJed441KpahS0ZO4v03MaptZi9ZBU7MvewL+sAU+cuZdO2oj1XxA8SU2uRse7Qfd93pG0iMTX5N8fHxMVx+jUXsnD8ZAAmvTCS5OaNeXzDDO6ZP4EPb3gA5wp+QZKEQ5lq1TmQfuiTxQPb0ilbrXq+MbEJCeTs3g3BYJ4x1QAol1KH2PjKnPjIUzT757+peva5xRe8lAqlpce2CXCFc+56M/sQ+PPvjG8BtAHKA8sJfV1bGzP7J3Atoa9wK/UK+7V7tG+isnNymDR3KTdcpF8Ox1Oh56qIH+Y0Sq1F/z924fon/kPF8uU4sV5tYmNLy3tSkeOv0Cu9j5CYXvnicJZNmcHyaTMBOLlHZ9LmLOKf51xBjUb1ueHrt3moVS/2Z+4+XiH7VqG/9wqcq8LHeOtiY6nYqAnL7r4VK1eWZk8+z56li8nasP44RBt9SnChNWxKS2K7yjk3x5ueDTT4nfHfOecygUwz2wl87i2fD7QsONjMBgADAP5983Vcd363cMR83NVKSmBTnqrf5oyd1EysfFTbmDp/Oc3r1aZ6lfhwhyd51EqqwqZtO3LnN2/fSY2khCI//89d2vPnLu0BePaj8fkq9SJ+1GXwNXS6/goA1sycS1LdlNx1iXWS2bFhc6HP+8O9NxBfoxrvDByYu+yMfpcw4bGXANi6Yg3pq9aR3KwRq2fOPY5H4B81ep9P9R69AdizbCllq9dkD6GL+8pWq86B7dvyjc/ZtZO4+HiIiYFgMN+YA9u2krNrJ8Gs/ZC1n90L5lGhYSMltkVkPvgkorSUfbLyTAcIJeQ5HIq//BHGB/PMBykkmXfOjXDOtXPOtSstSS1AiwYprNm8nbStGWTn5DBuxgK6tmp2VNsYN2M+vdqrDeF4a3FCHdZu3kba1u2hczV9Lme3KfrFett2hSpHG9MzmDhrAb3OaHW8QhUpFSa/+BYPt+nNw216M2fUV3S49iIAGp7ehv07M9m1aethz+n418s4qUdn/nvF/+VrNdi+dgPNuoV6OCvXrE5y0xPYunJt8RyID2wdO4bFNwxi8Q2D2DH9e6qdE/qEsFLT5gT27iEnY/thz8mcN5ekjqELaat1O4+dP/0AwM7pPxJ/8ikQE4OVK0elps3Yv07nSg4pLRXbwqwG2gIzgIsjG0pkxMXGcueVvRn07FsEgkEu7NiGxqk1+XBS6OO1S7ueRvrOTC57aAR79mURY8Zb30xn9INDiK9Qnn1ZB/hx0QruveZPET6S6BcXG8ud1/Zh4BP/JeCCXNj5NBrXSeaDb6cDcNk5HUjfkcll9z3P7n1ZxMQYb0+YxujH/kF8hfL8/fm32LF7L3Gxsdx17QVUqVQxwkfkb289OpDObZtSPTGeleOf4sGXR/PGqKmRDsu3Foz9jha9z2b48smh2331uyV33dAvX+et625j58YtXPnyw2xfs55bf/wMgF8+Hc/Y4c8zdvjz/OWNp7hn3ngw49PbHmPPtoxIHU5U2zVrBlXanU6LESO92309lbuu8X0Ps+Zfz5C9fRtpb7zKCbfeRcrVfdm3cgXpX40HYH/aWnbNnslJ/xoBLkj6V+PYv3Z1hI6mFHLBSEdw3FlJb5A3swbAF865Ft78zUA88D7wIbAb+Ba42jnXwMz6Au2cc0O98au9+fSC6wpzYOr7JfsHIrmsbMFCvZRUlQaNiXQIUkT953wb6RCkiK7/Y5NIhyBHoe3nX0e8wzVrT2bYcpxylSpH/HgKU+Irts651YQuBjs4/1Se1Xn7Ze/21r8BvJFnfIM80/nWiYiIiPiGDyq2paXHVkRERETkiEp8xVZEREREwqCEt5+GgxJbERERET9QK4KIiIiISOmgiq2IiIiID5gPKrZKbEVERET8wAeJrVoRRERERCQqqGIrIiIi4gc+qNgqsRURERHxAx8ktmpFEBEREZGooIqtiIiIiB8Eo79iq8RWRERExAf8cLsvtSKIiIiISFRQxVZERETED3xQsVViKyIiIuIHzkU6guNOrQgiIiIiEhVUsRURERHxA7UiiIiIiEg00F0RRERERERKCVVsRURERPzABxVbJbYiIiIifuCDxFatCCIiIiISFVSxFREREfEDH1RsldiKiIiI+IDuiiAiIiIiUkqoYisiIiLiB8Hor9gqsRURERHxA+ciHcFxp1YEEREREYkKqtiKiIiI+IEPLh5TYisiIiLiA7orgoiIiIhIKaGKrYiIiIgf+KBiq8S2gDEXD490CFJEO7MDkQ5Biqh/xv5IhyBF9FrrcyIdghRR8jdTIh2CHIW2kQ4AfJHYqhVBRERERKKCKrYiIiIifhCM/k86ldiKiIiI+IDzwTePqRVBRERERKKCKrYiIiIifqBWBBERERGJCj5IbNWKICIiIiJhZ2Y9zWypmS03s9sLWX+Vmc3zHj+YWatj3acqtiIiIiI+4ALFV7E1s1jg30B3IA2YaWZjnHOL8gxbBXRxzmWYWS9gBHD6sexXia2IiIiIHxTvXRHaA8udcysBzOx9oA+Qm9g6537IM346UOdYd6pWBBERERE5KmY2wMxm5XkMKDAkFViXZz7NW/Zb/gqMO9a4VLEVERER8YMwXjzmnBtBqHXgt1hhTyt0oNnZhBLbTscalxJbERERER9wxXtXhDSgbp75OsCGgoPMrCXwH6CXc27bse5UrQgiIiIiEm4zgSZm1tDMygKXA2PyDjCzesCnwDXOuV/DsVNVbEVERET8oBgvHnPO5ZjZUGACEAu85pxbaGaDvPUvA/cC1YAXzQwgxznX7lj2q8RWRERExAeKuRUB59xYYGyBZS/nmb4OuC6c+1QrgoiIiIhEBVVsRURERPzAB1+pq8RWRERExA+K9wsaIkKtCCIiIiISFVSxFREREfEBF1ArgoiIiIhEA/XYioiIiEhU8EFiqx5bEREREYkKqtiKiIiI+IDzwV0RlNiKiIiI+IFaEURERERESgdVbEVERET8wAcVWyW2IiIiIj7ghx5btSKIiIiISFRQxVZERETED9SKICIiIiJRwQeJrVoRRERERCQqqGIrIiIi4gMuEP0VWyW2IiIiIn6guyJIadDq4TvoOX0c5373KYmnNC90TKP+V9Jz+jgu3ryQslUT862rceZpnDvxE7pPHk2Xz944/gH7WIdH7+KSWRO4cOpoqrU8qdAxza+7iktmTeCv25dQLs+5KlslgW5v/osLp47m/K8/JKl5k2KK2p8ufe4+Hlw2ibvnjqNum5MLHdP/7We5f8lE7pk/gWv++wQxcaFaQfmEygwe8x/unjOOexd8xRl9LynO0CWPEff1I23is/zy0YORDkU8vZ6+h2ELJvK3GV9Qu3Xhr62LXn+aoXO/YvCssfR5+dHc19ZBKW1P4d7dSznpwp7FEbKUIsec2JrZWDNLPIrxDcxswbHu939hZrsjsd/jKbnbWVRuWJ/xHXrx8833c+oT9xY6btuMn5lyyV/Zs3Z9vuVlEirT5rF7+P7aoXzdpQ/Tr7+pOML2pTrndiahUX0+ateDaX+/lzOfvq/QcVt++plxF/Yns8C5anXTQLYvWMJnZ/Vh8uDb6PDIncURti+16NWVmk0acm+Trrwz4E6ufOnhQsfNeGcU9zfrxvBTelC2Qnk6XXc5AF2HXMPGRct5qHUvnul6ORc/fRexZcoU5yGI583Pv+ePQ56JdBjiadKjC1UbNeD5Ft34fOjd/OH5BwodN//9MbzQ6jxebNebuArlObXfpbnrLCaG7g/dyoqvpxZX2NEjGAjfo4Q65sTWOdfbObcjDLHI/yCl5zms+WgMANtnz6NMQmXK16x+2LgdC5awd92Gw5bXvegPrB/7DfvWbwQgK3378Q3Yx+r37sby90cDsHXWXMomJFChVo3Dxm2bv5jd69YftjypaSM2TP4RgJ3LVhFfL5XyNaod36B9qmWf85j+5qcArPrpFyokViYh+fBztWDcpNzp1TPmklQnGQDnoHzlSgCUi6/Inu07CObkHP/A5TDTfv6VjJ17Ih2GeJr+8VzmvvsZAGkz5lC+SgLxhby2lk2YnDu9ftY8ElKTc+dPH3wti0ZNYM/Wbcc/4CjjgoGwPUqq301szexWMxvmTf/TzL71pruZ2dtmttrMqnuV2MVm9qqZLTSzr8ysgje2rZnNNbMfgSF5tn2ymc0wszlmNs/MmnjbWWJmI71lH5tZxTzbmWxms81sgpnV9pY3MrPx3vKpZtbMW97QzH40s5lmNjzsP70SoELtmuxdvyl3ft/GzVSoXavIz6/cqAFlqiTQ5dPX6fbVh9S75PzjEaYAFWvXYo/3BgJg74ZNVDqKc7VtwVIa/Ok8AKqfegrxdVOolJL8O8+S/0Viai0y8rwR3JG2icTU3/5Zx8TFcfo1F7JwfOiP8aQXRpLcvDGPb5jBPfMn8OEND+CcO+5xi5R0CSm12JV26PfgrvWbSEj57d+DMXFxtLriApZ/PQWAyim1aHb+ecx69d3jHquUTkWp2E4BzvKm2wHxZlYG6AQU/BygCfBv59zJwA7gz97y14FhzrkzCowfBDznnGvtbTvNW94UGOGcawnsAgZ7+/wXcLFzri3wGnDw88ERwP95y28GXvSWPwe85Jw7DTiU/RVgZgPMbJaZzfp6X8bv/TxKGDtsydH8AbXYWJJancS0qwcz9fIBNL9pEPEn1A9ngOKxw0/VUZ2rec+NoGxiAhdM/oyTr7+abfMW41QFPC6s8JP1m+OvfHE4y6bMYPm0mQCc3KMzaXMWcVtKex5u3ZvLX3iQ8pXjj1e4IqVHIa+tI/0e/MNzD7Dm+xms/X4WAD2fvJtv7n7CF18Nezy4YDBsj5KqKHdFmA20NbPKQBbwM6Ek9CxgGHBHnrGrnHNz8jyvgZlVARKdcwc/V3gL6OVN/wjcZWZ1gE+dc8u8PyjrnHPfe2Pe9vYzHmgBfO2NiQU2mlk8cCbwUZ4/RuW8fztyKLl+C3i8sAN0zo0glBzzca2TS3xZpVG/K2h49cUAbJ+zgIqpyRz8QKZC7Vrs37SlyNvat3EzB7ZnENi7j8DefaRPn0WVk5uye+Wa4xC5/zT/65U0vTZ04VD6L/OplFo7d13FlGT2HsW5ys7cw9Shh/pqL50zkcy1aUd4hhyNLoOvodP1VwCwZuZckuqm5K5LrJPMjg2bC33eH+69gfga1Xhn4MDcZWf0u4QJj70EwNYVa0hftY7kZo1YPXPucTwCkZLptIFX09brkV0/ez4JdQ79HkxITSZzY+G/B7vc+X9UqlGVDy67O3dZyqktuPjNZwGoWC2JJj26EszJYcnn3xy/A4giLlByE9Jw+d3E1jmXbWargX7AD8A84GygEbC4wPCsPNMBoAKhkmKhyaJz7l0z+wn4AzDBzK4DVhYy3nnbWViw6mtmCcAOr+pb6G6OdHyl0YrX32PF6+8BkHxuZxr3v5J1n42latuWZGfuZv+W9CJva8P4b2nz6F1YbCwxZctQ9dSWLHvlzeMVuu8s/u+7LP5v6COzut270Pz6q1j56ZfUaNeK7F2Z7Nu8tcjbKptQmZx9+wlmZ9P02kvY9MNMsjPVOxguk198i8kvvgVAi95n03XoX5j1/hgant6G/Tsz2bXp8HPV8a+XcVKPzjzb7cp8VaftazfQrFtHlk+bSeWa1UluegJbV64ttmMRKUlmvvI2M195G4AmPbvSftA1LPjwC+q0b03Wrkx2F/LaOrXvpTTufhYje12T77X1XPOzc6cvGPE4v477Tkmt5FPUi8emEPqIfwqh9oNBwBxXhM9RvQvLdppZJ2/RVQfXmdkJwErn3PPAGKClt6qemR1MYK8ApgFLgRoHl5tZGTM72Tm3C1hlZpd4y83MWnnP/R64vOB+o8mmb6awe00aPX8aR9unH+CX2w61End85yXKexcnNb7uKnr/MpEKKbXo/t1ntH0mdCVq5rKVbPp2Gt2/+4xzxr/Pqnc+YdeS5RE5lmi37uvJZK5exyWzv6LTs8P54ZZDtx8674NXqJhcE4CTBlzD5QsmUSmlFhdOHUOn50LnNLFpIy764XP+PH0sdbqdxfQ7HonIcfjBgrHfkb5yLcOXT+bqVx/l3cH35K4b+uXrVKkdOldXvvwwCbWqc+uPn3HXL2Ppfc8wAMYOf54TzjyVe+aN58aJ7/DpbY+xZ1tpa3OKDm89OpApI+/ixPrJrBz/FH0vOOv3nyTHzbLxk8hYtY5hC7/lT/9+mC9vOHR3mKs++w+VvdfWH//1IJVqVue6SR8xaPoYutwxNFIhRxUXCIbtUVJZUXr8zKwboVaAROfcHjP7FXjZOfeMV81tB8QDXzjnWnjPuRmId87db2YHe2L3AhMI9cm2MLM7gKuBbEI9sFcCCcBYQkn0mcAy4Brn3F4zaw08D1QhVG1+1jn3qpk1BF4CagNlgPedcw96y9/1xn4C3O2cO2KjW2loRZCQndkl96pMyW9mxv5IhyBF9FrrcyIdghTRnUumRDoEOQr371teSPN+8dr537vDluNU+etDET+ewhTpm8eccxMJJYwH50/MM93Am0wn1AN7cPlTeaZnAwerqAD3e8sfBR7Nuy+vtSDonBtUSBxzgM6FLF8FHHaXZm953taFxw4/OhERERGJBvpKXREREREfKMktBOFS4hJb59xq8lR+RUREROTY+SGxPeZvHhMRERERKQlKXMVWRERERMIvGIj+i66V2IqIiIj4QEn+xrBwUSuCiIiIiEQFVWxFREREfMAPF48psRURERHxAT8ktmpFEBEREZGooIqtiIiIiA/44eIxJbYiIiIiPhBUK4KIiIiISOmgiq2IiIiID/jh4jEltiIiIiI+4IfEVq0IIiIiIhIVVLEVERER8QHdFUFEREREooJaEURERERESglVbEVERER8wA8VWyW2IiIiIj4Q9EGPrVoRRERERCQqKLEVERER8QEXCIbtURRm1tPMlprZcjO7vZD1ZmbPe+vnmdmpx3qMakUQERER8QEXCBTbvswsFvg30B1IA2aa2Rjn3KI8w3oBTbzH6cBL3r//M1VsRURERCTc2gPLnXMrnXMHgPeBPgXG9AHedCHTgUQzq30sO1ViKyIiIuIDLhgM28PMBpjZrDyPAQV2lwqsyzOf5i072jFHRa0IIiIiIj4Qztt9OedGACOOMMQKe9r/MOaoqGIrIiIiIuGWBtTNM18H2PA/jDkqSmxFREREfKCY74owE2hiZg3NrCxwOTCmwJgxwLXe3RE6ADudcxuP5RjViiAiIiLiA8Fi/OYx51yOmQ0FJgCxwGvOuYVmNshb/zIwFugNLAf2Av2Odb9KbEVEREQk7JxzYwklr3mXvZxn2gFDwrlPJbYiIiIiPuB88JW6SmwLWPXh6EiHIEUUCB7ThZNSjK5/ZnCkQ5AiSv5mSqRDkCJ6pFnnSIcgR+H+SAdAeO+KUFIpsRURERHxAReI/oKQ7oogIiIiIlFBFVsRERERHyjOuyJEihJbERERER9wPrg2Ra0IIiIiIhIVVLEVERER8YGgDy4eU2IrIiIi4gN+uN2XWhFEREREJCqoYisiIiLiA364j60SWxEREREf8EOPrVoRRERERCQqqGIrIiIi4gN+uHhMia2IiIiIDwT1BQ0iIiIiIqWDKrYiIiIiPqC7IoiIiIhIVAj6oMdWrQgiIiIiEhVUsRURERHxAbUiiIiIiEhU8ENiq1YEEREREYkKqtiKiIiI+IAfLh5TYisiIiLiA05f0CAiIiIiUjqoYisiIiLiA0EfXDymxFZERETEB5wPemzViiAiIiIiUUEVWxEREREf8MN9bJXYioiIiPiAH3ps1YogIiIiIlFBFVsRERERH3DB6L94TImtiIiIiA+oFUFEREREpJRQxVZERETEB3RXBBERERGJCn74ggYltqXcugWzmP7BCFwwSNNO59Gq16WHjdmwdB7TPxhBMBCgfHwCf7zlcXKyD/Dlk7cRyMkmGAjQsG1H2p5/dQSOwD/SFs7mpw9D5+rEjufRsuclh43ZuHQeMz56lWAgQLn4BHr/4zFysg8w7qnQuXLBIA1O7UibP10VgSPwl7oDBpPQtj3BrCxWP/ck+1YsP2xM2VrJnHDLncRWTmDvimWsfuZxXE4OAPEtWlL3+sFYXCw5u3bx6x3/KO5D8JVeT99Dkx5dyd67j1EDbmPjnIWHjbno9adJOfUUgtk5rJ81l8+H3kPQO18AKW1P4brJH/PxNTew6LPxxRm+ACPu60fvzq3Yun0XbS65N9LhSCnli8TWzBoAZzrn3o10LOEUDAb44d2X6PX3h6iUVJ3Rj/ydeq06kJRSL3dM1t7d/PDui/Qc9iDx1Wqyb9cOAGLjytD7pkcoU74CwZwcPn/iFuq2aEfNE5pF6GiiWzAYYPp7L9HjhoeomFSNzx/9O/Vank5igXP143svcd6wB4ivmv9c9fy7d64COXz55K2kntxW5+o4SmjbnnIpqSwc2JdKTZtT/2/DWHLzsMPGpfa9js2jPyVj6iTqDb6Bat17kj7uC2IrVaLe34ax7P47yN66lbgqicV+DH7SpEcXqjZqwPMtulGnfWv+8PwD/KfzxYeNm//+GD7tF3qD8eeR/+TUfpcy69XQnwWLiaH7Q7ey4uupxRq7HPLm59/z4gcTeX34dZEOJWrp4rHo0QC4MtJBhNvWVb+SUDOFhBq1iY0rwwmndWbN3On5xqyYMYkGbc4kvlpNACokJAJgZpQpXwGAYCCHYCBQrLH7TfrqX6lcszaVayTnnqu18/Kfq5UzJlO/zZnEV/39c2VmxRq/3yR2OINt334DwJ6li4mtFE9cUtXDxiW0bE3G91MA2DbxKxI7dASgapdz2PHjNLK3bgUgZ+eO4gncp5r+8VzmvvsZAGkz5lC+SgLxyTUOG7dswuTc6fWz5pGQmpw7f/rga1k0agJ7tm47/gFLoab9/CsZO/dEOoyo5gIubI+SqlRXbM3sWuBmwAHzgACwC2gHJAO3Ouc+Bh4DmpvZHGCkc+6fkYk4vPbu2EalqtVz5yslVmfrqqX5xuzcvIFgIIcvnrqd7P17adGtD03O6AaEqoijHrqBXVs3clLXP6gCeBztzdhGpaRDf2grFnKudm1ZTzAQYNzTt5OdtY+Tzjmfxh0OnavPH7mRXVs30qzLH6jRsGmxxu83ZapV50D6ltz5A9vSKVutOjkZ23OXxSYkkLN7N3j3hQyNqQZAuZQ6WFwcJz7yFDEVKrBlzGds/+6b4j0IH0lIqcWutI2587vWbyIhpRa7N20tdHxMXBytrriAcbcMB6BySi2anX8eI3teTWrbR4slZhE5PkptYmtmJwN3AR2dc+lmVhV4BqgNdAKaAWOAj4HbgZudc3/8jW0NAAYAXPSP4XT40+XFcATHzrlC3jEVKOS5QID0NcvpfdMjBA5kMebxm6l5QjOq1EolJiaWi+59gay9u/nmxYfYvn41VVMbFEvsflPYe9uCVddgIMC2tcvpcePDBLKz+OLxm6nR8NC56nP3v8jau5tvX36YjPWrSdK5Om6s4AsJoMDrrfAx3rrYWCo2asKyu2/FypWl2ZPPs2fpYrI2rD8O0QqFfIJR6O9Hzx+ee4A1389g7fezAOj55N18c/cTvrh5vfhb8Aivi2hRahNb4BzgY+dcOoBzbruXKIxyzgWBRWZWqygbcs6NAEYAPDl5eak565WSqrNne3ru/J4d6VRMrFZgTDXKxSdQplx5ypQrT3KTk9m2biVVaqXmjilXMZ7aTVuStnC2EtvjpFJSNfZkHKoe7d2RTsXEqgXGVKd8vnPVgu1pqw47V8knnkLawp+V2IZZjd7nU71HbwD2LFtK2eo12UPoAqSy1apzYHv+j6hzdu0kLj4eYmIgGMw35sC2reTs2kkwaz9k7Wf3gnlUaNhIiW0YnTbwatr2C10su372fBLq1M5dl5CaTObGLYU+r8ud/0elGlX54LK7c5elnNqCi998FoCK1ZJo0qMrwZwclnyuKrtEl4APEtvS3GNrFF4IyyowJmrVaHAiu7asJzN9E4GcbFbOnEL9VqfnG1OvdQc2L19IMBAgJ2s/W1f9SmLtuuzL3EnW3t0A5BzIYv3iOSQm143EYfhC9fonsmvLhnznqm7LAueqVZ5zdWA/W1cvJTG5DvsLnKuNS+aQmFwnEocR1baOHcPiGwax+IZB7Jj+PdXOOReASk2bE9i7J18bwkGZ8+aS1LEzANW6ncfOn34AYOf0H4k/+RSIicHKlaNS02bsX7e2+A7GB2a+8jYvdziflzucz5LPv6bVlRcCUKd9a7J2ZRbahnBq30tp3P0sPr72xnwV3eean82zzbrybLOuLPpsPF/eeJ+SWpFSqjRXbCcCn5nZP51z27xWhN+SCVQupriKTUxsLGde8TfGPXuPdwup7iSl1Gfx5LEANO/Sm6Ta9ahzcls+fXAIZjE07XQeVVMbsC1tFVNef4ZgMAjO0bBdJ+q1bB/hI4peMbGxdLhsEF89fy8uGKTJmaFztWRK6Fw169ybxNp1ST25LaOGD8VijBM79iAptQHb01YxdeQ/ccEgzgVp2PYs6upcHVe7Zs2gSrvTaTFipHe7r6dy1zW+72HW/OsZsrdvI+2NVznh1rtIubov+1auIP2r0C2i9qetZdfsmZz0rxHggqR/NY79a1dH6Gii37Lxk2jSoyvDFn5L9t59jB54W+66qz77D2MG30nmxi388V8PsmPtBq6b9BEAi0d/xeRHX4hU2FLAW48OpHPbplRPjGfl+Kd48OXRvDFKd6kIpxJ8zVfY2JH6kEo6M/sLcAuhi8Z+8RZ/4V0whpntds7Fm1kZYDxQHXjjSBePlaZWBL8LBHWqSovuzwyOdAhSRJ9/syrSIUgRPdKsc6RDkKNw4JfXIv4p8gc1TwrbH87LtiyK+PEUpjRXbHHOjQRGHmF9vPdvNtCtuOISERERkeJXqhNbERERESkaP7QiKLEVERER8QHdFUFEREREpJRQxVZERETEB9SKICIiIiJRQa0IIiIiIiJhZGZVzexrM1vm/ZtUyJi6ZvadmS02s4VmdkNRtq3EVkRERMQHAi58j2N0OzDROdeE0Bdu3V7ImBzgH8655kAHYIiZnfR7G1ZiKyIiIuIDJSix7cOh7yEYCVxQcIBzbqNz7mdvOhNYDKT+3oaV2IqIiIjIUTGzAWY2K89jwFE8vZZzbiOEElig5u/sqwHQBvjp9zasi8dEREREfCCcF48550YAI35rvZl9AyQXsuquo9mPmcUDnwA3Oud2/d54JbYiIiIiPlCct/tyzp37W+vMbLOZ1XbObTSz2sCW3xhXhlBS+45z7tOi7FetCCIiIiI+EHAubI9jNAb4izf9F2B0wQFmZsB/gcXOuWeKumEltiIiIiJSnB4DupvZMqC7N4+ZpZjZWG9MR+Aa4Bwzm+M9ev/ehtWKICIiIuIDJeWbx5xz24BuhSzfAPT2pqcBdrTbVmIrIiIi4gP65jERERERkVJCFVsRERERHygprQjHkxJbERERER9QK4KIiIiISCmhiq2IiIiIDwQjHUAxUGIrIiIi4gNqRRARERERKSVUsRURERHxAd0VQURERESigloRRERERERKCVVsRURERHxArQgiIiIiEhXUiiAiIiIiUkqoYisiIiLiA2pFEBEREZGooFYEEREREZFSQhVbERERER/wQyuCOR+UpQXMbIBzbkSk45Dfp3NVeuhclR46V6WHzpUcC7Ui+MeASAcgRaZzVXroXJUeOlelh86V/M+U2IqIiIhIVFBiKyIiIiJRQYmtf6hfqfTQuSo9dK5KD52r0kPnSv5nunhMRERERKKCKrYiIiIiEhWU2IqIiIhIVFBiWwqZWaKZDY50HHJ0zOzOSMcgRWNmDcxsQaTjEDCzsWaWeBTjI3buzGx3JPYb7bxzemWk45DSQYlt6ZQIKLEtfZTYihwl51xv59yOSMchEdUAUGIrRaLEtnR6DGhkZnPM7Ekzu8XMZprZPDN7AHLf4S4xs/+Y2QIze8fMzjWz781smZm198bdb2Zvmdm33vLrI3pkUcLMRpnZbDNbaGYDzOwxoIJ3zt7xxlxtZjO8Za+YWay3fLeZPe49/xsza29mk8xspZmd743pa2ajzWy8mS01s/sieLjRKtbMXvXO4VdmVsE7D+0AzKy6ma32pvt65/xzM1tlZkPN7CYz+8XMpptZ1YgeSQlmZrea2TBv+p9m9q033c3M3jaz1d7PuoGZLS54Tryxbc1srpn9CAzJs+2T87zG5plZkzy/G0d6yz42s4p5tjPZe+1NMLPa3vJG3mtttplNNbNm3vKGZvaj9/t3eDH/6Eo9M7vWOwdzvb9Db5jZ82b2g/f77mJv6GPAWd55/HskY5ZSwDmnRyl7EHr3usCbPo/QrVGM0BuVL4DO3pgc4BRv+WzgNW9cH2CU9/z7gblABaA6sA5IifQxlvYHUNX7twKwAKgG7M6zvjnwOVDGm38RuNabdkAvb/oz4CugDNAKmOMt7wts9LZ7cB/tIn3c0fLI8/pp7c1/CFwNTDr4c/ZeL6vznI/lQGWgBrATGOSt+ydwY6SPqaQ+gA7AR970VGCG9//9PmAgsNr7WRd6TrzpeUAXb/rJPL8f/wVc5U2X9V4rDbzXWEdv+WvAzd4+fwBqeMsvA17zpicCTbzp04FvvekxeV63Q/K+xvX43fN+MrAUqO7NVwXeAD4i9DfrJGC5t64r8EWkY9ajdDzikNLuPO/xizcfDzQB1gKrnHPzAcxsITDROefMbD6hX+4HjXbO7QP2mdl3QHtgVPGEH7WGmdmF3nRdQuckr25AW2CmmUHoD+4Wb90BYLw3PR/Ics5lF3LevnbObQMws0+BTsCsMB+Hn61yzs3xpmeT/2dfmO+cc5lAppntJPTGBULnsOVxiTA6zAbamlllIAv4GWgHnAUMA+7IM/awc2JmVYBE59xkb/lbQC9v+kfgLjOrA3zqnFvmvd7WOee+98a87e1nPNAC+NobEwtsNLN44EzgI285QDnv347An/Ps9/Fj+Dn4zTnAx865dADn3Hbv5zvKORcEFplZrUgGKKWTEtvSz4BHnXOv5Fto1oDQH4mDgnnmg+Q/9wVvZqybGx8DM+sKnAuc4Zzba2aTgPIFhwEjnXN3cLhs59zBc5B73pxzQTPTeSs+eV8/AUJvPnI41MJV8JwW9fUmeXhv2lYD/QhVTOcBZwONgMUFhhd2Tozf+L/vnHvXzH4C/gBMMLPrgJWFjHfedhY6587Iu8LMEoAdzrnWv3UIRzo++U2/dd6yCowROSrqsS2dMgl95AkwAejvVRUws1Qzq3mU2+tjZuXNrBqhj3xmhi1Sf6oCZHhJbTNCH7UCZJtZGW96InDxwXNlZlXNrP5R7qe797wKwAXA978zXo7dakKVdoCLjzBOjs4UQu0AUwi1Iwwi1Hbzu0mjC11YttPMOnmLrjq4zsxOAFY6554n1DZwsHJez8wOJrBXANMIfSxe4+ByMytjZic753YBq8zsEm+5mVkr77nfA5cX3K8UyUTgUu/vDr/Th573b57IESmxLYW8j5+/t9AtbboD7wI/eh9Vf8zR/wKYAXwJTAeGO+c2hDNeHxoPxJnZPGA4oZ8rhHqh55nZO865RcDdwFfeuK+B2ke5n2mEPv6cA3zinFMbwvH3FPA3M/uBUN+nhMdUQv//f3TObQb2e8uKqh/wb+/isX15ll8GLDCzOUAz4E1v+WLgL95rryrwknPuAKE3K4+b2VxCr6szvfFXAX/1li8kdJ0CwA3AEDObSegNrRSRc24h8DAw2fu5PnOE4fOAHO8iM108Jkekr9T1OTO7n9AFD09FOhYpOjPrS+gipqGRjkWkNPHatL5wzrWIdCwiEn6q2IqIiIhIVFDFVkRERESigiq2IiIiIhIVlNiKiIiISFRQYisiIiIiUUGJrYiIiIhEBSW2IiIiIhIV/h9i8onLoFet0AAAAABJRU5ErkJggg==\n",
      "text/plain": [
       "<Figure size 864x504 with 2 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "plt.figure(figsize = (12,7))\n",
    "sns.heatmap(bike_num.corr(), annot = True, cmap=\"RdBu\")\n",
    "#plt.xticks(rotation = 90)\n",
    "plt.yticks(rotation = 0)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The above correlation plot tells us that there is a high correlation between 'temp','atemp' vs 'cnt'"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > RESCALING THE FEATURES"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 44,
   "metadata": {},
   "outputs": [],
   "source": [
    "from sklearn.preprocessing import MinMaxScaler"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 45,
   "metadata": {},
   "outputs": [],
   "source": [
    "scaler = MinMaxScaler()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 46,
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
       "      <th>yr</th>\n",
       "      <th>holiday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>cnt</th>\n",
       "      <th>season_Spring</th>\n",
       "      <th>season_Summer</th>\n",
       "      <th>...</th>\n",
       "      <th>mnth_Oct</th>\n",
       "      <th>mnth_Sep</th>\n",
       "      <th>weekday_1</th>\n",
       "      <th>weekday_2</th>\n",
       "      <th>weekday_3</th>\n",
       "      <th>weekday_4</th>\n",
       "      <th>weekday_5</th>\n",
       "      <th>weekday_6</th>\n",
       "      <th>weathersit_Light_Snow_Rain</th>\n",
       "      <th>weathersit_Mist_Cloudy</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>483</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>18.791653</td>\n",
       "      <td>22.50605</td>\n",
       "      <td>58.7083</td>\n",
       "      <td>7.832836</td>\n",
       "      <td>6304</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>650</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>16.126653</td>\n",
       "      <td>19.56980</td>\n",
       "      <td>49.4583</td>\n",
       "      <td>9.791514</td>\n",
       "      <td>7109</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>212</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>31.638347</td>\n",
       "      <td>35.16460</td>\n",
       "      <td>55.0833</td>\n",
       "      <td>10.500039</td>\n",
       "      <td>4266</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>714</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>14.862500</td>\n",
       "      <td>18.49690</td>\n",
       "      <td>83.8750</td>\n",
       "      <td>6.749714</td>\n",
       "      <td>3786</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>5.671653</td>\n",
       "      <td>5.80875</td>\n",
       "      <td>43.4167</td>\n",
       "      <td>24.250650</td>\n",
       "      <td>822</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows Ã— 30 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     yr  holiday  workingday       temp     atemp      hum  windspeed   cnt  \\\n",
       "483   1        0           1  18.791653  22.50605  58.7083   7.832836  6304   \n",
       "650   1        0           0  16.126653  19.56980  49.4583   9.791514  7109   \n",
       "212   0        0           1  31.638347  35.16460  55.0833  10.500039  4266   \n",
       "714   1        0           1  14.862500  18.49690  83.8750   6.749714  3786   \n",
       "8     0        0           1   5.671653   5.80875  43.4167  24.250650   822   \n",
       "\n",
       "     season_Spring  season_Summer  ...  mnth_Oct  mnth_Sep  weekday_1  \\\n",
       "483              0              1  ...         0         0          1   \n",
       "650              0              0  ...         1         0          0   \n",
       "212              0              0  ...         0         0          0   \n",
       "714              0              0  ...         0         0          1   \n",
       "8                1              0  ...         0         0          0   \n",
       "\n",
       "     weekday_2  weekday_3  weekday_4  weekday_5  weekday_6  \\\n",
       "483          0          0          0          0          0   \n",
       "650          0          0          0          0          0   \n",
       "212          0          1          0          0          0   \n",
       "714          0          0          0          0          0   \n",
       "8            1          0          0          0          0   \n",
       "\n",
       "     weathersit_Light_Snow_Rain  weathersit_Mist_Cloudy  \n",
       "483                           0                       0  \n",
       "650                           0                       0  \n",
       "212                           0                       0  \n",
       "714                           0                       1  \n",
       "8                             0                       0  \n",
       "\n",
       "[5 rows x 30 columns]"
      ]
     },
     "execution_count": 46,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Checking the values before scaling\n",
    "df_train.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 47,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['yr', 'holiday', 'workingday', 'temp', 'atemp', 'hum', 'windspeed',\n",
       "       'cnt', 'season_Spring', 'season_Summer', 'season_Winter', 'mnth_Aug',\n",
       "       'mnth_Dec', 'mnth_Feb', 'mnth_Jan', 'mnth_Jul', 'mnth_Jun', 'mnth_Mar',\n",
       "       'mnth_May', 'mnth_Nov', 'mnth_Oct', 'mnth_Sep', 'weekday_1',\n",
       "       'weekday_2', 'weekday_3', 'weekday_4', 'weekday_5', 'weekday_6',\n",
       "       'weathersit_Light_Snow_Rain', 'weathersit_Mist_Cloudy'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 47,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_train.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 48,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Apply scaler() to all the numeric variables\n",
    "\n",
    "num_vars = ['temp', 'atemp', 'hum', 'windspeed','cnt']\n",
    "\n",
    "df_train[num_vars] = scaler.fit_transform(df_train[num_vars])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 49,
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
       "      <th>yr</th>\n",
       "      <th>holiday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>cnt</th>\n",
       "      <th>season_Spring</th>\n",
       "      <th>season_Summer</th>\n",
       "      <th>...</th>\n",
       "      <th>mnth_Oct</th>\n",
       "      <th>mnth_Sep</th>\n",
       "      <th>weekday_1</th>\n",
       "      <th>weekday_2</th>\n",
       "      <th>weekday_3</th>\n",
       "      <th>weekday_4</th>\n",
       "      <th>weekday_5</th>\n",
       "      <th>weekday_6</th>\n",
       "      <th>weathersit_Light_Snow_Rain</th>\n",
       "      <th>weathersit_Mist_Cloudy</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>483</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.497426</td>\n",
       "      <td>0.487055</td>\n",
       "      <td>0.609956</td>\n",
       "      <td>0.194850</td>\n",
       "      <td>0.722734</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>650</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0.416433</td>\n",
       "      <td>0.409971</td>\n",
       "      <td>0.513852</td>\n",
       "      <td>0.255118</td>\n",
       "      <td>0.815347</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>212</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.887856</td>\n",
       "      <td>0.819376</td>\n",
       "      <td>0.572294</td>\n",
       "      <td>0.276919</td>\n",
       "      <td>0.488265</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>714</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.378013</td>\n",
       "      <td>0.381804</td>\n",
       "      <td>0.871429</td>\n",
       "      <td>0.161523</td>\n",
       "      <td>0.433042</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.098690</td>\n",
       "      <td>0.048706</td>\n",
       "      <td>0.451083</td>\n",
       "      <td>0.700017</td>\n",
       "      <td>0.092039</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows Ã— 30 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     yr  holiday  workingday      temp     atemp       hum  windspeed  \\\n",
       "483   1        0           1  0.497426  0.487055  0.609956   0.194850   \n",
       "650   1        0           0  0.416433  0.409971  0.513852   0.255118   \n",
       "212   0        0           1  0.887856  0.819376  0.572294   0.276919   \n",
       "714   1        0           1  0.378013  0.381804  0.871429   0.161523   \n",
       "8     0        0           1  0.098690  0.048706  0.451083   0.700017   \n",
       "\n",
       "          cnt  season_Spring  season_Summer  ...  mnth_Oct  mnth_Sep  \\\n",
       "483  0.722734              0              1  ...         0         0   \n",
       "650  0.815347              0              0  ...         1         0   \n",
       "212  0.488265              0              0  ...         0         0   \n",
       "714  0.433042              0              0  ...         0         0   \n",
       "8    0.092039              1              0  ...         0         0   \n",
       "\n",
       "     weekday_1  weekday_2  weekday_3  weekday_4  weekday_5  weekday_6  \\\n",
       "483          1          0          0          0          0          0   \n",
       "650          0          0          0          0          0          0   \n",
       "212          0          0          1          0          0          0   \n",
       "714          1          0          0          0          0          0   \n",
       "8            0          1          0          0          0          0   \n",
       "\n",
       "     weathersit_Light_Snow_Rain  weathersit_Mist_Cloudy  \n",
       "483                           0                       0  \n",
       "650                           0                       0  \n",
       "212                           0                       0  \n",
       "714                           0                       1  \n",
       "8                             0                       0  \n",
       "\n",
       "[5 rows x 30 columns]"
      ]
     },
     "execution_count": 49,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Checking values after scaling\n",
    "df_train.head()"
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
       "      <th>yr</th>\n",
       "      <th>holiday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>cnt</th>\n",
       "      <th>season_Spring</th>\n",
       "      <th>season_Summer</th>\n",
       "      <th>...</th>\n",
       "      <th>mnth_Oct</th>\n",
       "      <th>mnth_Sep</th>\n",
       "      <th>weekday_1</th>\n",
       "      <th>weekday_2</th>\n",
       "      <th>weekday_3</th>\n",
       "      <th>weekday_4</th>\n",
       "      <th>weekday_5</th>\n",
       "      <th>weekday_6</th>\n",
       "      <th>weathersit_Light_Snow_Rain</th>\n",
       "      <th>weathersit_Mist_Cloudy</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.00000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "      <td>510.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>0.501961</td>\n",
       "      <td>0.023529</td>\n",
       "      <td>0.692157</td>\n",
       "      <td>0.540901</td>\n",
       "      <td>0.515631</td>\n",
       "      <td>0.647390</td>\n",
       "      <td>0.346318</td>\n",
       "      <td>0.515144</td>\n",
       "      <td>0.24902</td>\n",
       "      <td>0.247059</td>\n",
       "      <td>...</td>\n",
       "      <td>0.084314</td>\n",
       "      <td>0.082353</td>\n",
       "      <td>0.143137</td>\n",
       "      <td>0.152941</td>\n",
       "      <td>0.131373</td>\n",
       "      <td>0.139216</td>\n",
       "      <td>0.147059</td>\n",
       "      <td>0.143137</td>\n",
       "      <td>0.025490</td>\n",
       "      <td>0.341176</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>0.500487</td>\n",
       "      <td>0.151726</td>\n",
       "      <td>0.462054</td>\n",
       "      <td>0.227898</td>\n",
       "      <td>0.213626</td>\n",
       "      <td>0.149722</td>\n",
       "      <td>0.160266</td>\n",
       "      <td>0.224281</td>\n",
       "      <td>0.43287</td>\n",
       "      <td>0.431725</td>\n",
       "      <td>...</td>\n",
       "      <td>0.278131</td>\n",
       "      <td>0.275172</td>\n",
       "      <td>0.350557</td>\n",
       "      <td>0.360284</td>\n",
       "      <td>0.338139</td>\n",
       "      <td>0.346511</td>\n",
       "      <td>0.354512</td>\n",
       "      <td>0.350557</td>\n",
       "      <td>0.157763</td>\n",
       "      <td>0.474570</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.00000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.343228</td>\n",
       "      <td>0.335807</td>\n",
       "      <td>0.536147</td>\n",
       "      <td>0.230784</td>\n",
       "      <td>0.359468</td>\n",
       "      <td>0.00000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.540519</td>\n",
       "      <td>0.525578</td>\n",
       "      <td>0.646367</td>\n",
       "      <td>0.325635</td>\n",
       "      <td>0.516337</td>\n",
       "      <td>0.00000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.740406</td>\n",
       "      <td>0.692378</td>\n",
       "      <td>0.757900</td>\n",
       "      <td>0.434287</td>\n",
       "      <td>0.685861</td>\n",
       "      <td>0.00000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.00000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>8 rows Ã— 30 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "               yr     holiday  workingday        temp       atemp         hum  \\\n",
       "count  510.000000  510.000000  510.000000  510.000000  510.000000  510.000000   \n",
       "mean     0.501961    0.023529    0.692157    0.540901    0.515631    0.647390   \n",
       "std      0.500487    0.151726    0.462054    0.227898    0.213626    0.149722   \n",
       "min      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "25%      0.000000    0.000000    0.000000    0.343228    0.335807    0.536147   \n",
       "50%      1.000000    0.000000    1.000000    0.540519    0.525578    0.646367   \n",
       "75%      1.000000    0.000000    1.000000    0.740406    0.692378    0.757900   \n",
       "max      1.000000    1.000000    1.000000    1.000000    1.000000    1.000000   \n",
       "\n",
       "        windspeed         cnt  season_Spring  season_Summer  ...    mnth_Oct  \\\n",
       "count  510.000000  510.000000      510.00000     510.000000  ...  510.000000   \n",
       "mean     0.346318    0.515144        0.24902       0.247059  ...    0.084314   \n",
       "std      0.160266    0.224281        0.43287       0.431725  ...    0.278131   \n",
       "min      0.000000    0.000000        0.00000       0.000000  ...    0.000000   \n",
       "25%      0.230784    0.359468        0.00000       0.000000  ...    0.000000   \n",
       "50%      0.325635    0.516337        0.00000       0.000000  ...    0.000000   \n",
       "75%      0.434287    0.685861        0.00000       0.000000  ...    0.000000   \n",
       "max      1.000000    1.000000        1.00000       1.000000  ...    1.000000   \n",
       "\n",
       "         mnth_Sep   weekday_1   weekday_2   weekday_3   weekday_4   weekday_5  \\\n",
       "count  510.000000  510.000000  510.000000  510.000000  510.000000  510.000000   \n",
       "mean     0.082353    0.143137    0.152941    0.131373    0.139216    0.147059   \n",
       "std      0.275172    0.350557    0.360284    0.338139    0.346511    0.354512   \n",
       "min      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "25%      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "50%      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "75%      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "max      1.000000    1.000000    1.000000    1.000000    1.000000    1.000000   \n",
       "\n",
       "        weekday_6  weathersit_Light_Snow_Rain  weathersit_Mist_Cloudy  \n",
       "count  510.000000                  510.000000              510.000000  \n",
       "mean     0.143137                    0.025490                0.341176  \n",
       "std      0.350557                    0.157763                0.474570  \n",
       "min      0.000000                    0.000000                0.000000  \n",
       "25%      0.000000                    0.000000                0.000000  \n",
       "50%      0.000000                    0.000000                0.000000  \n",
       "75%      0.000000                    0.000000                1.000000  \n",
       "max      1.000000                    1.000000                1.000000  \n",
       "\n",
       "[8 rows x 30 columns]"
      ]
     },
     "execution_count": 50,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_train.describe()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > BUILDING A LINEAR MODEL\n",
    "\n",
    "Dividing into X and Y sets for the model building\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 51,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "483    0.722734\n",
       "650    0.815347\n",
       "212    0.488265\n",
       "714    0.433042\n",
       "8      0.092039\n",
       "         ...   \n",
       "467    0.733548\n",
       "444    0.714220\n",
       "46     0.240796\n",
       "374    0.411413\n",
       "366    0.221928\n",
       "Name: cnt, Length: 510, dtype: float64"
      ]
     },
     "execution_count": 51,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "y_train = df_train.pop('cnt')\n",
    "X_train = df_train\n",
    "y_train"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**RFE**(\n",
    "Recursive feature elimination): We will be using the LinearRegression function from SciKit Learn for its compatibility with RFE (which is a utility from sklearn)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 52,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Importing RFE and LinearRegression\n",
    "from sklearn.feature_selection import RFE\n",
    "from sklearn.linear_model import LinearRegression"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Running RFE with the output number of the variable equal to 15\n",
    "lm = LinearRegression()\n",
    "lm.fit(X_train, y_train)\n",
    "\n",
    "rfe = RFE(lm, 15)             # running RFE\n",
    "rfe = rfe.fit(X_train, y_train)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[('yr', True, 1),\n",
       " ('holiday', False, 10),\n",
       " ('workingday', False, 7),\n",
       " ('temp', True, 1),\n",
       " ('atemp', True, 1),\n",
       " ('hum', True, 1),\n",
       " ('windspeed', True, 1),\n",
       " ('season_Spring', True, 1),\n",
       " ('season_Summer', False, 13),\n",
       " ('season_Winter', True, 1),\n",
       " ('mnth_Aug', False, 11),\n",
       " ('mnth_Dec', True, 1),\n",
       " ('mnth_Feb', True, 1),\n",
       " ('mnth_Jan', True, 1),\n",
       " ('mnth_Jul', True, 1),\n",
       " ('mnth_Jun', False, 14),\n",
       " ('mnth_Mar', False, 9),\n",
       " ('mnth_May', False, 8),\n",
       " ('mnth_Nov', True, 1),\n",
       " ('mnth_Oct', False, 12),\n",
       " ('mnth_Sep', True, 1),\n",
       " ('weekday_1', False, 3),\n",
       " ('weekday_2', False, 2),\n",
       " ('weekday_3', False, 4),\n",
       " ('weekday_4', False, 6),\n",
       " ('weekday_5', False, 5),\n",
       " ('weekday_6', False, 15),\n",
       " ('weathersit_Light_Snow_Rain', True, 1),\n",
       " ('weathersit_Mist_Cloudy', True, 1)]"
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "list(zip(X_train.columns,rfe.support_,rfe.ranking_))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['yr', 'temp', 'atemp', 'hum', 'windspeed', 'season_Spring',\n",
       "       'season_Winter', 'mnth_Dec', 'mnth_Feb', 'mnth_Jan', 'mnth_Jul',\n",
       "       'mnth_Nov', 'mnth_Sep', 'weathersit_Light_Snow_Rain',\n",
       "       'weathersit_Mist_Cloudy'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 55,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "col = X_train.columns[rfe.support_]\n",
    "col"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['holiday', 'workingday', 'season_Summer', 'mnth_Aug', 'mnth_Jun',\n",
       "       'mnth_Mar', 'mnth_May', 'mnth_Oct', 'weekday_1', 'weekday_2',\n",
       "       'weekday_3', 'weekday_4', 'weekday_5', 'weekday_6'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 56,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "X_train.columns[~rfe.support_]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 57,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Creating X_test dataframe with RFE selected variables\n",
    "X_train_rfe = X_train[col]"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' >Building Linear Model using STATS MODEL\n",
    "## <span style='background: lightGreen '> ***Model - 1***  </span>  <br>\n",
    "VIF Check"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Check for the VIF values of the feature variables. \n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
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
       "      <th>Features</th>\n",
       "      <th>VIF</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>temp</td>\n",
       "      <td>353.54</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>atemp</td>\n",
       "      <td>352.02</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>hum</td>\n",
       "      <td>23.93</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>windspeed</td>\n",
       "      <td>5.15</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>season_Spring</td>\n",
       "      <td>4.56</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>season_Winter</td>\n",
       "      <td>2.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>mnth_Jan</td>\n",
       "      <td>2.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>weathersit_Mist_Cloudy</td>\n",
       "      <td>2.29</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>mnth_Feb</td>\n",
       "      <td>2.28</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>yr</td>\n",
       "      <td>2.05</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>mnth_Nov</td>\n",
       "      <td>1.87</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>mnth_Dec</td>\n",
       "      <td>1.64</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>mnth_Jul</td>\n",
       "      <td>1.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>weathersit_Light_Snow_Rain</td>\n",
       "      <td>1.23</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>mnth_Sep</td>\n",
       "      <td>1.22</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      Features     VIF\n",
       "1                         temp  353.54\n",
       "2                        atemp  352.02\n",
       "3                          hum   23.93\n",
       "4                    windspeed    5.15\n",
       "5                season_Spring    4.56\n",
       "6                season_Winter    2.77\n",
       "9                     mnth_Jan    2.42\n",
       "14      weathersit_Mist_Cloudy    2.29\n",
       "8                     mnth_Feb    2.28\n",
       "0                           yr    2.05\n",
       "11                    mnth_Nov    1.87\n",
       "7                     mnth_Dec    1.64\n",
       "10                    mnth_Jul    1.42\n",
       "13  weathersit_Light_Snow_Rain    1.23\n",
       "12                    mnth_Sep    1.22"
      ]
     },
     "execution_count": 59,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Create a dataframe that will contain the names of all the feature variables and their respective VIFs\n",
    "vif = pd.DataFrame()\n",
    "vif['Features'] = X_train_rfe.columns\n",
    "vif['VIF'] = [variance_inflation_factor(X_train_rfe.values, i) for i in range(X_train_rfe.shape[1])]\n",
    "vif['VIF'] = round(vif['VIF'], 2)\n",
    "vif = vif.sort_values(by = \"VIF\", ascending = False)\n",
    "vif"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 60,
   "metadata": {},
   "outputs": [],
   "source": [
    "import statsmodels.api as sm"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 61,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Add a constant\n",
    "X_train_lm1 = sm.add_constant(X_train_rfe)\n",
    "\n",
    "# Create a first fitted model\n",
    "lr1 = sm.OLS(y_train, X_train_lm1).fit()"
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
       "const                         0.392882\n",
       "yr                            0.230435\n",
       "temp                          0.293277\n",
       "atemp                         0.133309\n",
       "hum                          -0.170856\n",
       "windspeed                    -0.182617\n",
       "season_Spring                -0.088663\n",
       "season_Winter                 0.076156\n",
       "mnth_Dec                     -0.069304\n",
       "mnth_Feb                     -0.049005\n",
       "mnth_Jan                     -0.068918\n",
       "mnth_Jul                     -0.049823\n",
       "mnth_Nov                     -0.071463\n",
       "mnth_Sep                      0.059465\n",
       "weathersit_Light_Snow_Rain   -0.264079\n",
       "weathersit_Mist_Cloudy       -0.047365\n",
       "dtype: float64"
      ]
     },
     "execution_count": 62,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check the parameters obtained\n",
    "\n",
    "lr1.params"
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
      "                            OLS Regression Results                            \n",
      "==============================================================================\n",
      "Dep. Variable:                    cnt   R-squared:                       0.836\n",
      "Model:                            OLS   Adj. R-squared:                  0.831\n",
      "Method:                 Least Squares   F-statistic:                     168.4\n",
      "Date:                Mon, 10 May 2021   Prob (F-statistic):          3.83e-183\n",
      "Time:                        19:53:59   Log-Likelihood:                 500.95\n",
      "No. Observations:                 510   AIC:                            -969.9\n",
      "Df Residuals:                     494   BIC:                            -902.2\n",
      "Df Model:                          15                                         \n",
      "Covariance Type:            nonrobust                                         \n",
      "==============================================================================================\n",
      "                                 coef    std err          t      P>|t|      [0.025      0.975]\n",
      "----------------------------------------------------------------------------------------------\n",
      "const                          0.3929      0.033     11.763      0.000       0.327       0.459\n",
      "yr                             0.2304      0.008     27.757      0.000       0.214       0.247\n",
      "temp                           0.2933      0.131      2.243      0.025       0.036       0.550\n",
      "atemp                          0.1333      0.138      0.968      0.333      -0.137       0.404\n",
      "hum                           -0.1709      0.038     -4.481      0.000      -0.246      -0.096\n",
      "windspeed                     -0.1826      0.028     -6.452      0.000      -0.238      -0.127\n",
      "season_Spring                 -0.0887      0.019     -4.748      0.000      -0.125      -0.052\n",
      "season_Winter                  0.0762      0.014      5.327      0.000       0.048       0.104\n",
      "mnth_Dec                      -0.0693      0.019     -3.646      0.000      -0.107      -0.032\n",
      "mnth_Feb                      -0.0490      0.022     -2.272      0.023      -0.091      -0.007\n",
      "mnth_Jan                      -0.0689      0.022     -3.118      0.002      -0.112      -0.025\n",
      "mnth_Jul                      -0.0498      0.017     -2.866      0.004      -0.084      -0.016\n",
      "mnth_Nov                      -0.0715      0.019     -3.713      0.000      -0.109      -0.034\n",
      "mnth_Sep                       0.0595      0.016      3.788      0.000       0.029       0.090\n",
      "weathersit_Light_Snow_Rain    -0.2641      0.029     -9.151      0.000      -0.321      -0.207\n",
      "weathersit_Mist_Cloudy        -0.0474      0.011     -4.369      0.000      -0.069      -0.026\n",
      "==============================================================================\n",
      "Omnibus:                       85.349   Durbin-Watson:                   2.041\n",
      "Prob(Omnibus):                  0.000   Jarque-Bera (JB):              217.893\n",
      "Skew:                          -0.841   Prob(JB):                     4.84e-48\n",
      "Kurtosis:                       5.724   Cond. No.                         75.7\n",
      "==============================================================================\n",
      "\n",
      "Notes:\n",
      "[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.\n"
     ]
    }
   ],
   "source": [
    "# Print a summary of the linear regression model obtained\n",
    "print(lr1.summary())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style='background: lightGreen '> ***Model - 2***  </span>  <br>\n",
    "* Removing the variable 'atemp' based on its Very High 'VIF' value.<br>\n",
    "* Even though the VIF of atemp is second highest, we decided to drop 'atemp' and not 'temp' based on general knowledge that temperature can be an important factor for a business like bike rentals, and wanted to retain 'temp'.<br> "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 64,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train_new = X_train_rfe.drop([\"atemp\"], axis = 1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "VIF Check"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 65,
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
       "      <th>Features</th>\n",
       "      <th>VIF</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>hum</td>\n",
       "      <td>23.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>temp</td>\n",
       "      <td>16.95</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>windspeed</td>\n",
       "      <td>5.04</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>season_Spring</td>\n",
       "      <td>4.55</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>season_Winter</td>\n",
       "      <td>2.74</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>mnth_Jan</td>\n",
       "      <td>2.41</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>mnth_Feb</td>\n",
       "      <td>2.28</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>weathersit_Mist_Cloudy</td>\n",
       "      <td>2.28</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>yr</td>\n",
       "      <td>2.05</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>mnth_Nov</td>\n",
       "      <td>1.87</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>mnth_Dec</td>\n",
       "      <td>1.64</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>mnth_Jul</td>\n",
       "      <td>1.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>weathersit_Light_Snow_Rain</td>\n",
       "      <td>1.23</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>mnth_Sep</td>\n",
       "      <td>1.21</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      Features    VIF\n",
       "2                          hum  23.42\n",
       "1                         temp  16.95\n",
       "3                    windspeed   5.04\n",
       "4                season_Spring   4.55\n",
       "5                season_Winter   2.74\n",
       "8                     mnth_Jan   2.41\n",
       "7                     mnth_Feb   2.28\n",
       "13      weathersit_Mist_Cloudy   2.28\n",
       "0                           yr   2.05\n",
       "10                    mnth_Nov   1.87\n",
       "6                     mnth_Dec   1.64\n",
       "9                     mnth_Jul   1.42\n",
       "12  weathersit_Light_Snow_Rain   1.23\n",
       "11                    mnth_Sep   1.21"
      ]
     },
     "execution_count": 65,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check for the VIF values of the feature variables. \n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "\n",
    "# Create a dataframe that will contain the names of all the feature variables and their respective VIFs\n",
    "vif = pd.DataFrame()\n",
    "vif['Features'] = X_train_new.columns\n",
    "vif['VIF'] = [variance_inflation_factor(X_train_new.values, i) for i in range(X_train_new.shape[1])]\n",
    "vif['VIF'] = round(vif['VIF'], 2)\n",
    "vif = vif.sort_values(by = \"VIF\", ascending = False)\n",
    "vif"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 66,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Add a constant\n",
    "X_train_lm2 = sm.add_constant(X_train_new)\n",
    "\n",
    "# Create a first fitted model\n",
    "lr2 = sm.OLS(y_train, X_train_lm2).fit()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 67,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "const                         0.395902\n",
       "yr                            0.230468\n",
       "temp                          0.415618\n",
       "hum                          -0.168786\n",
       "windspeed                    -0.187299\n",
       "season_Spring                -0.088553\n",
       "season_Winter                 0.077183\n",
       "mnth_Dec                     -0.069832\n",
       "mnth_Feb                     -0.049238\n",
       "mnth_Jan                     -0.070386\n",
       "mnth_Jul                     -0.049787\n",
       "mnth_Nov                     -0.072215\n",
       "mnth_Sep                      0.058620\n",
       "weathersit_Light_Snow_Rain   -0.265242\n",
       "weathersit_Mist_Cloudy       -0.047723\n",
       "dtype: float64"
      ]
     },
     "execution_count": 67,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "lr2.params"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 68,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "                            OLS Regression Results                            \n",
      "==============================================================================\n",
      "Dep. Variable:                    cnt   R-squared:                       0.836\n",
      "Model:                            OLS   Adj. R-squared:                  0.832\n",
      "Method:                 Least Squares   F-statistic:                     180.4\n",
      "Date:                Mon, 10 May 2021   Prob (F-statistic):          4.46e-184\n",
      "Time:                        19:54:01   Log-Likelihood:                 500.47\n",
      "No. Observations:                 510   AIC:                            -970.9\n",
      "Df Residuals:                     495   BIC:                            -907.4\n",
      "Df Model:                          14                                         \n",
      "Covariance Type:            nonrobust                                         \n",
      "==============================================================================================\n",
      "                                 coef    std err          t      P>|t|      [0.025      0.975]\n",
      "----------------------------------------------------------------------------------------------\n",
      "const                          0.3959      0.033     11.907      0.000       0.331       0.461\n",
      "yr                             0.2305      0.008     27.763      0.000       0.214       0.247\n",
      "temp                           0.4156      0.034     12.344      0.000       0.349       0.482\n",
      "hum                           -0.1688      0.038     -4.434      0.000      -0.244      -0.094\n",
      "windspeed                     -0.1873      0.028     -6.717      0.000      -0.242      -0.133\n",
      "season_Spring                 -0.0886      0.019     -4.743      0.000      -0.125      -0.052\n",
      "season_Winter                  0.0772      0.014      5.414      0.000       0.049       0.105\n",
      "mnth_Dec                      -0.0698      0.019     -3.676      0.000      -0.107      -0.033\n",
      "mnth_Feb                      -0.0492      0.022     -2.284      0.023      -0.092      -0.007\n",
      "mnth_Jan                      -0.0704      0.022     -3.192      0.002      -0.114      -0.027\n",
      "mnth_Jul                      -0.0498      0.017     -2.864      0.004      -0.084      -0.016\n",
      "mnth_Nov                      -0.0722      0.019     -3.755      0.000      -0.110      -0.034\n",
      "mnth_Sep                       0.0586      0.016      3.740      0.000       0.028       0.089\n",
      "weathersit_Light_Snow_Rain    -0.2652      0.029     -9.199      0.000      -0.322      -0.209\n",
      "weathersit_Mist_Cloudy        -0.0477      0.011     -4.404      0.000      -0.069      -0.026\n",
      "==============================================================================\n",
      "Omnibus:                       84.095   Durbin-Watson:                   2.042\n",
      "Prob(Omnibus):                  0.000   Jarque-Bera (JB):              214.671\n",
      "Skew:                          -0.830   Prob(JB):                     2.43e-47\n",
      "Kurtosis:                       5.711   Cond. No.                         19.0\n",
      "==============================================================================\n",
      "\n",
      "Notes:\n",
      "[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.\n"
     ]
    }
   ],
   "source": [
    "# Print a summary of the linear regression model obtained\n",
    "print(lr2.summary())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style='background: lightGreen '> ***Model - 3***  </span>  <br>\n",
    "* Removing the variable 'hum' based on its Very High 'VIF' value."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 69,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train_new = X_train_new.drop([\"hum\"], axis = 1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "VIF Check"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 70,
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
       "      <th>Features</th>\n",
       "      <th>VIF</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>temp</td>\n",
       "      <td>5.24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>windspeed</td>\n",
       "      <td>5.03</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>season_Spring</td>\n",
       "      <td>4.20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>season_Winter</td>\n",
       "      <td>2.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>mnth_Jan</td>\n",
       "      <td>2.30</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>mnth_Feb</td>\n",
       "      <td>2.24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>yr</td>\n",
       "      <td>2.04</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>mnth_Nov</td>\n",
       "      <td>1.82</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>weathersit_Mist_Cloudy</td>\n",
       "      <td>1.53</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>mnth_Dec</td>\n",
       "      <td>1.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>mnth_Jul</td>\n",
       "      <td>1.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>mnth_Sep</td>\n",
       "      <td>1.21</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>weathersit_Light_Snow_Rain</td>\n",
       "      <td>1.07</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      Features   VIF\n",
       "1                         temp  5.24\n",
       "2                    windspeed  5.03\n",
       "3                season_Spring  4.20\n",
       "4                season_Winter  2.42\n",
       "7                     mnth_Jan  2.30\n",
       "6                     mnth_Feb  2.24\n",
       "0                           yr  2.04\n",
       "9                     mnth_Nov  1.82\n",
       "12      weathersit_Mist_Cloudy  1.53\n",
       "5                     mnth_Dec  1.52\n",
       "8                     mnth_Jul  1.37\n",
       "10                    mnth_Sep  1.21\n",
       "11  weathersit_Light_Snow_Rain  1.07"
      ]
     },
     "execution_count": 70,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check for the VIF values of the feature variables. \n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "\n",
    "# Create a dataframe that will contain the names of all the feature variables and their respective VIFs\n",
    "vif = pd.DataFrame()\n",
    "vif['Features'] = X_train_new.columns\n",
    "vif['VIF'] = [variance_inflation_factor(X_train_new.values, i) for i in range(X_train_new.shape[1])]\n",
    "vif['VIF'] = round(vif['VIF'], 2)\n",
    "vif = vif.sort_values(by = \"VIF\", ascending = False)\n",
    "vif"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 71,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Add a constant\n",
    "X_train_lm3 = sm.add_constant(X_train_new)\n",
    "\n",
    "# Create a first fitted model\n",
    "lr3 = sm.OLS(y_train, X_train_lm3).fit()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 72,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "const                         0.303644\n",
       "yr                            0.234951\n",
       "temp                          0.382729\n",
       "windspeed                    -0.152288\n",
       "season_Spring                -0.086553\n",
       "season_Winter                 0.071272\n",
       "mnth_Dec                     -0.082184\n",
       "mnth_Feb                     -0.054138\n",
       "mnth_Jan                     -0.076869\n",
       "mnth_Jul                     -0.042346\n",
       "mnth_Nov                     -0.073365\n",
       "mnth_Sep                      0.053151\n",
       "weathersit_Light_Snow_Rain   -0.315355\n",
       "weathersit_Mist_Cloudy       -0.075665\n",
       "dtype: float64"
      ]
     },
     "execution_count": 72,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check the parameters obtained\n",
    "\n",
    "lr3.params"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 73,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "                            OLS Regression Results                            \n",
      "==============================================================================\n",
      "Dep. Variable:                    cnt   R-squared:                       0.830\n",
      "Model:                            OLS   Adj. R-squared:                  0.825\n",
      "Method:                 Least Squares   F-statistic:                     185.8\n",
      "Date:                Mon, 10 May 2021   Prob (F-statistic):          4.71e-181\n",
      "Time:                        19:54:02   Log-Likelihood:                 490.54\n",
      "No. Observations:                 510   AIC:                            -953.1\n",
      "Df Residuals:                     496   BIC:                            -893.8\n",
      "Df Model:                          13                                         \n",
      "Covariance Type:            nonrobust                                         \n",
      "==============================================================================================\n",
      "                                 coef    std err          t      P>|t|      [0.025      0.975]\n",
      "----------------------------------------------------------------------------------------------\n",
      "const                          0.3036      0.026     11.493      0.000       0.252       0.356\n",
      "yr                             0.2350      0.008     27.994      0.000       0.218       0.251\n",
      "temp                           0.3827      0.033     11.440      0.000       0.317       0.448\n",
      "windspeed                     -0.1523      0.027     -5.590      0.000      -0.206      -0.099\n",
      "season_Spring                 -0.0866      0.019     -4.552      0.000      -0.124      -0.049\n",
      "season_Winter                  0.0713      0.014      4.929      0.000       0.043       0.100\n",
      "mnth_Dec                      -0.0822      0.019     -4.293      0.000      -0.120      -0.045\n",
      "mnth_Feb                      -0.0541      0.022     -2.468      0.014      -0.097      -0.011\n",
      "mnth_Jan                      -0.0769      0.022     -3.430      0.001      -0.121      -0.033\n",
      "mnth_Jul                      -0.0423      0.018     -2.403      0.017      -0.077      -0.008\n",
      "mnth_Nov                      -0.0734      0.020     -3.746      0.000      -0.112      -0.035\n",
      "mnth_Sep                       0.0532      0.016      3.340      0.001       0.022       0.084\n",
      "weathersit_Light_Snow_Rain    -0.3154      0.027    -11.671      0.000      -0.368      -0.262\n",
      "weathersit_Mist_Cloudy        -0.0757      0.009     -8.427      0.000      -0.093      -0.058\n",
      "==============================================================================\n",
      "Omnibus:                       85.688   Durbin-Watson:                   2.024\n",
      "Prob(Omnibus):                  0.000   Jarque-Bera (JB):              223.666\n",
      "Skew:                          -0.837   Prob(JB):                     2.70e-49\n",
      "Kurtosis:                       5.779   Cond. No.                         15.3\n",
      "==============================================================================\n",
      "\n",
      "Notes:\n",
      "[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.\n"
     ]
    }
   ],
   "source": [
    "# Print a summary of the linear regression model obtained\n",
    "print(lr3.summary())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style='background: lightGreen '> ***Model - 4***  </span>  <br>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* Removing the variable windspeed based on its Very High 'VIF' value.\n",
    "* Even though the VIF of windspeed is second highest, we decided to drop 'windspeed' and not 'temp' based on general knowledge that temperature can be an important factor for a business like bike rentals, and wanted to retain 'temp'.<br> "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 74,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train_new = X_train_new.drop(['windspeed'], axis = 1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "VIF Check"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 75,
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
       "      <th>Features</th>\n",
       "      <th>VIF</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>season_Spring</td>\n",
       "      <td>3.95</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>temp</td>\n",
       "      <td>2.88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>season_Winter</td>\n",
       "      <td>2.42</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>mnth_Jan</td>\n",
       "      <td>2.27</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>mnth_Feb</td>\n",
       "      <td>2.22</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>yr</td>\n",
       "      <td>2.03</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>mnth_Nov</td>\n",
       "      <td>1.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>weathersit_Mist_Cloudy</td>\n",
       "      <td>1.52</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>mnth_Dec</td>\n",
       "      <td>1.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>mnth_Jul</td>\n",
       "      <td>1.34</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>mnth_Sep</td>\n",
       "      <td>1.20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>weathersit_Light_Snow_Rain</td>\n",
       "      <td>1.05</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      Features   VIF\n",
       "2                season_Spring  3.95\n",
       "1                         temp  2.88\n",
       "3                season_Winter  2.42\n",
       "6                     mnth_Jan  2.27\n",
       "5                     mnth_Feb  2.22\n",
       "0                           yr  2.03\n",
       "8                     mnth_Nov  1.77\n",
       "11      weathersit_Mist_Cloudy  1.52\n",
       "4                     mnth_Dec  1.51\n",
       "7                     mnth_Jul  1.34\n",
       "9                     mnth_Sep  1.20\n",
       "10  weathersit_Light_Snow_Rain  1.05"
      ]
     },
     "execution_count": 75,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check for the VIF values of the feature variables. \n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "\n",
    "# Create a dataframe that will contain the names of all the feature variables and their respective VIFs\n",
    "vif = pd.DataFrame()\n",
    "vif['Features'] = X_train_new.columns\n",
    "vif['VIF'] = [variance_inflation_factor(X_train_new.values, i) for i in range(X_train_new.shape[1])]\n",
    "vif['VIF'] = round(vif['VIF'], 2)\n",
    "vif = vif.sort_values(by = \"VIF\", ascending = False)\n",
    "vif"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 76,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Add a constant\n",
    "X_train_lm4 = sm.add_constant(X_train_new)\n",
    "\n",
    "# Create a first fitted model\n",
    "lr4 = sm.OLS(y_train, X_train_lm4).fit()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 77,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "const                         0.238038\n",
       "yr                            0.232633\n",
       "temp                          0.400895\n",
       "season_Spring                -0.086655\n",
       "season_Winter                 0.084050\n",
       "mnth_Dec                     -0.076714\n",
       "mnth_Feb                     -0.055132\n",
       "mnth_Jan                     -0.076232\n",
       "mnth_Jul                     -0.035965\n",
       "mnth_Nov                     -0.079272\n",
       "mnth_Sep                      0.059580\n",
       "weathersit_Light_Snow_Rain   -0.331546\n",
       "weathersit_Mist_Cloudy       -0.073761\n",
       "dtype: float64"
      ]
     },
     "execution_count": 77,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check the parameters obtained\n",
    "\n",
    "lr4.params"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 78,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "                            OLS Regression Results                            \n",
      "==============================================================================\n",
      "Dep. Variable:                    cnt   R-squared:                       0.819\n",
      "Model:                            OLS   Adj. R-squared:                  0.815\n",
      "Method:                 Least Squares   F-statistic:                     187.3\n",
      "Date:                Mon, 10 May 2021   Prob (F-statistic):          1.19e-175\n",
      "Time:                        19:54:04   Log-Likelihood:                 474.96\n",
      "No. Observations:                 510   AIC:                            -923.9\n",
      "Df Residuals:                     497   BIC:                            -868.9\n",
      "Df Model:                          12                                         \n",
      "Covariance Type:            nonrobust                                         \n",
      "==============================================================================================\n",
      "                                 coef    std err          t      P>|t|      [0.025      0.975]\n",
      "----------------------------------------------------------------------------------------------\n",
      "const                          0.2380      0.024      9.763      0.000       0.190       0.286\n",
      "yr                             0.2326      0.009     26.944      0.000       0.216       0.250\n",
      "temp                           0.4009      0.034     11.689      0.000       0.334       0.468\n",
      "season_Spring                 -0.0867      0.020     -4.425      0.000      -0.125      -0.048\n",
      "season_Winter                  0.0841      0.015      5.716      0.000       0.055       0.113\n",
      "mnth_Dec                      -0.0767      0.020     -3.896      0.000      -0.115      -0.038\n",
      "mnth_Feb                      -0.0551      0.023     -2.440      0.015      -0.100      -0.011\n",
      "mnth_Jan                      -0.0762      0.023     -3.303      0.001      -0.122      -0.031\n",
      "mnth_Jul                      -0.0360      0.018     -1.986      0.048      -0.072      -0.000\n",
      "mnth_Nov                      -0.0793      0.020     -3.935      0.000      -0.119      -0.040\n",
      "mnth_Sep                       0.0596      0.016      3.644      0.000       0.027       0.092\n",
      "weathersit_Light_Snow_Rain    -0.3315      0.028    -11.982      0.000      -0.386      -0.277\n",
      "weathersit_Mist_Cloudy        -0.0738      0.009     -7.981      0.000      -0.092      -0.056\n",
      "==============================================================================\n",
      "Omnibus:                       88.667   Durbin-Watson:                   2.026\n",
      "Prob(Omnibus):                  0.000   Jarque-Bera (JB):              244.514\n",
      "Skew:                          -0.846   Prob(JB):                     8.03e-54\n",
      "Kurtosis:                       5.940   Cond. No.                         14.5\n",
      "==============================================================================\n",
      "\n",
      "Notes:\n",
      "[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.\n"
     ]
    }
   ],
   "source": [
    "# Print a summary of the linear regression model obtained\n",
    "print(lr4.summary())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style='background: lightGreen '> ***Model - 5***  </span>  <br>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* All Variables VIF values are well below 5. The 'mnth_Jul' variable having its High P-value 0.048 which is close to 0.05 hence for safety purpose I drop this variable."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 79,
   "metadata": {},
   "outputs": [],
   "source": [
    "X_train_new = X_train_new.drop([\"mnth_Jul\"], axis = 1)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "VIF Check"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 80,
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
       "      <th>Features</th>\n",
       "      <th>VIF</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>season_Spring</td>\n",
       "      <td>3.92</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>season_Winter</td>\n",
       "      <td>2.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>temp</td>\n",
       "      <td>2.30</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>mnth_Jan</td>\n",
       "      <td>2.26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>mnth_Feb</td>\n",
       "      <td>2.22</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>yr</td>\n",
       "      <td>2.03</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>mnth_Nov</td>\n",
       "      <td>1.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>mnth_Dec</td>\n",
       "      <td>1.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>weathersit_Mist_Cloudy</td>\n",
       "      <td>1.50</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>mnth_Sep</td>\n",
       "      <td>1.17</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>weathersit_Light_Snow_Rain</td>\n",
       "      <td>1.05</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      Features   VIF\n",
       "2                season_Spring  3.92\n",
       "3                season_Winter  2.37\n",
       "1                         temp  2.30\n",
       "6                     mnth_Jan  2.26\n",
       "5                     mnth_Feb  2.22\n",
       "0                           yr  2.03\n",
       "7                     mnth_Nov  1.77\n",
       "4                     mnth_Dec  1.51\n",
       "10      weathersit_Mist_Cloudy  1.50\n",
       "8                     mnth_Sep  1.17\n",
       "9   weathersit_Light_Snow_Rain  1.05"
      ]
     },
     "execution_count": 80,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check for the VIF values of the feature variables. \n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "\n",
    "# Create a dataframe that will contain the names of all the feature variables and their respective VIFs\n",
    "vif = pd.DataFrame()\n",
    "vif['Features'] = X_train_new.columns\n",
    "vif['VIF'] = [variance_inflation_factor(X_train_new.values, i) for i in range(X_train_new.shape[1])]\n",
    "vif['VIF'] = round(vif['VIF'], 2)\n",
    "vif = vif.sort_values(by = \"VIF\", ascending = False)\n",
    "vif"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 81,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Add a constant\n",
    "X_train_lm5 = sm.add_constant(X_train_new)\n",
    "\n",
    "# Create a first fitted model\n",
    "lr5 = sm.OLS(y_train, X_train_lm5).fit()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 82,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "const                         0.248891\n",
       "yr                            0.232965\n",
       "temp                          0.375922\n",
       "season_Spring                -0.087867\n",
       "season_Winter                 0.084976\n",
       "mnth_Dec                     -0.080011\n",
       "mnth_Feb                     -0.057742\n",
       "mnth_Jan                     -0.080914\n",
       "mnth_Nov                     -0.082132\n",
       "mnth_Sep                      0.065011\n",
       "weathersit_Light_Snow_Rain   -0.333164\n",
       "weathersit_Mist_Cloudy       -0.072447\n",
       "dtype: float64"
      ]
     },
     "execution_count": 82,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check the parameters obtained\n",
    "\n",
    "lr5.params"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 83,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "                            OLS Regression Results                            \n",
      "==============================================================================\n",
      "Dep. Variable:                    cnt   R-squared:                       0.817\n",
      "Model:                            OLS   Adj. R-squared:                  0.813\n",
      "Method:                 Least Squares   F-statistic:                     202.8\n",
      "Date:                Mon, 10 May 2021   Prob (F-statistic):          5.79e-176\n",
      "Time:                        19:54:07   Log-Likelihood:                 472.94\n",
      "No. Observations:                 510   AIC:                            -921.9\n",
      "Df Residuals:                     498   BIC:                            -871.1\n",
      "Df Model:                          11                                         \n",
      "Covariance Type:            nonrobust                                         \n",
      "==============================================================================================\n",
      "                                 coef    std err          t      P>|t|      [0.025      0.975]\n",
      "----------------------------------------------------------------------------------------------\n",
      "const                          0.2489      0.024     10.444      0.000       0.202       0.296\n",
      "yr                             0.2330      0.009     26.908      0.000       0.216       0.250\n",
      "temp                           0.3759      0.032     11.747      0.000       0.313       0.439\n",
      "season_Spring                 -0.0879      0.020     -4.475      0.000      -0.126      -0.049\n",
      "season_Winter                  0.0850      0.015      5.765      0.000       0.056       0.114\n",
      "mnth_Dec                      -0.0800      0.020     -4.066      0.000      -0.119      -0.041\n",
      "mnth_Feb                      -0.0577      0.023     -2.553      0.011      -0.102      -0.013\n",
      "mnth_Jan                      -0.0809      0.023     -3.514      0.000      -0.126      -0.036\n",
      "mnth_Nov                      -0.0821      0.020     -4.075      0.000      -0.122      -0.043\n",
      "mnth_Sep                       0.0650      0.016      4.021      0.000       0.033       0.097\n",
      "weathersit_Light_Snow_Rain    -0.3332      0.028    -12.011      0.000      -0.388      -0.279\n",
      "weathersit_Mist_Cloudy        -0.0724      0.009     -7.836      0.000      -0.091      -0.054\n",
      "==============================================================================\n",
      "Omnibus:                       91.053   Durbin-Watson:                   2.008\n",
      "Prob(Omnibus):                  0.000   Jarque-Bera (JB):              250.513\n",
      "Skew:                          -0.869   Prob(JB):                     4.00e-55\n",
      "Kurtosis:                       5.961   Cond. No.                         13.6\n",
      "==============================================================================\n",
      "\n",
      "Notes:\n",
      "[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.\n"
     ]
    }
   ],
   "source": [
    "# Print a summary of the linear regression model obtained\n",
    "print(lr5.summary())"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "* This model looks good, as there seems to be VERY LOW Multicollinearity between the predictors and the p-values for all the predictors seems to be significant. For now, we will consider this as our final model (unless the Test data metrics are not significantly close to this number)."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > Final Model Interpretation <br>\n",
    "Hypothesis Testing:<br>\n",
    "Hypothesis testing states that:<br>\n",
    "\n",
    "H0:B1=B2=...=Bn=0<br>\n",
    "H1: at least one Bi!=0"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "lr6 model coefficient values"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "const                `=`          0.248891<br>\n",
    "yr                       `=`      0.232965<br>\n",
    "temp                         `=`  0.375922<br>\n",
    "season_Spring      `=`           -0.087867<br>\n",
    "season_Winter          `=`        0.084976<br>\n",
    "mnth_Dec                   `=`   -0.080011<br>\n",
    "mnth_Feb              `=`        -0.057742<br>\n",
    "mnth_Jan                  `=`    -0.080914<br>\n",
    "mnth_Nov                     `=` -0.082132<br>\n",
    "mnth_Sep              `=`         0.065011<br>\n",
    "weathersit_Light_Snow_Rain `=`   -0.333164<br>\n",
    "weathersit_Mist_Cloudy        `=`-0.072447<br>"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "* From the lr5 model summary, it is evident that all our coefficients are not equal to zerowhich means We REJECT the NULL HYPOTHESIS"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "F Statistics"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "F-Statistics is used for testing the overall significance of the Model: Higher the F-Statistics, more significant the Model is.\n",
    "\n",
    "*  F-statistic:                     202.8\n",
    "* Prob (F-statistic):          5.79e-176<br>\n",
    "The F-Statistics value are 202.8 (which is greater than 1) and the p-value of all the variables are '0.000' Except 'mnth_Feb' = 0.011 which is also well below 0.05 it states that the overall model is significant. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## The equation of best fitted surface based on model lr6:"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**cnt = 0.248891 + (yr Ã— 0.232965) + (temp Ã— 0.375922) - (season_Spring Ã— 0.087867) + (season_Winter Ã— 0.084976) - (mnth_Dec Ã— 0.080011) - (mnth_Feb Ã— 0.057742) - (mnth_Jan Ã— 0.080914) - (mnth_Nov Ã— 0.082132) + (mnth_Sep Ã— 0.065011) âˆ’ (weathersit_Light_Snow_Rain Ã— 0.333164) âˆ’ (weathersit_Mist_Cloudy Ã— 0.072447)**"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style='background: lightGreen '> Interpretation of Coefficients:  </span>  <br>\n",
    "\n",
    "This is similar to equation: Y = B0 + B1*x1 + B2*X2 ...Bn*Xn <br>\n",
    "where:<br>\n",
    "* If Positive sign: A coefficients value of (B1,B2,B3...Bn) indicated that a unit increase in Independent variable(X1,X2,X3...Xn), increases the bike hire numbers by (B1,B2,B3...Bn) units.\n",
    "\n",
    "\n",
    "* If Negative sign: A coefficients value of (B1,B2,B3...Bn) indicated that a unit increase in Independent variable(X1,X2,X3...Xn), decreases the bike hire numbers by (B1,B2,B3...Bn) units.\n",
    "\n",
    "\n",
    "* const: The Constant value of â€˜0.248891â€™ indicated that, in the absence of all other predictor variables <br> (i.e. when x1,x2...xn =0), The bike rental can still increase by 0.248891 units.\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## <span style='background: lightGreen '> ASSUMPTIONS  </span>  <br> "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Error terms are normally distributed with mean zero (not X, Y) <br>\n",
    "* Residual Analysis Of Training Data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 84,
   "metadata": {},
   "outputs": [],
   "source": [
    "y_train_pred = lr5.predict(X_train_lm5)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 85,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Text(0.5, 0, 'Errors')"
      ]
     },
     "execution_count": 85,
     "metadata": {},
     "output_type": "execute_result"
    },
    {
     "data": {
      "image/png": "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\n",
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
    "res = y_train-y_train_pred\n",
    "# Plot the histogram of the error terms\n",
    "fig = plt.figure()\n",
    "sns.distplot((res), bins = 20)\n",
    "fig.suptitle('Error Terms', fontsize = 20)                  # Plot heading \n",
    "plt.xlabel('Errors', fontsize = 18)                         # X-label"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "* From the above histogram, we could see that the Residuals are normally distributed. Hence our assumption for Linear Regression is valid."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### There is a linear relationship between X and Y"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 86,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<Figure size 900x900 with 30 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "bike_new=bike_new[[ 'temp', 'atemp', 'hum', 'windspeed','cnt']]\n",
    "\n",
    "sns.pairplot(bike_num, diag_kind='kde')\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "* Using the pair plot, we could see there is a linear relation between temp and atemp variable with the predictor â€˜cntâ€™."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "### There is No Multicollinearity between the predictor variables"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 87,
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
       "      <th>Features</th>\n",
       "      <th>VIF</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>season_Spring</td>\n",
       "      <td>3.92</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>season_Winter</td>\n",
       "      <td>2.37</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>temp</td>\n",
       "      <td>2.30</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>mnth_Jan</td>\n",
       "      <td>2.26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>mnth_Feb</td>\n",
       "      <td>2.22</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>yr</td>\n",
       "      <td>2.03</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>mnth_Nov</td>\n",
       "      <td>1.77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>mnth_Dec</td>\n",
       "      <td>1.51</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>weathersit_Mist_Cloudy</td>\n",
       "      <td>1.50</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>mnth_Sep</td>\n",
       "      <td>1.17</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>weathersit_Light_Snow_Rain</td>\n",
       "      <td>1.05</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "                      Features   VIF\n",
       "2                season_Spring  3.92\n",
       "3                season_Winter  2.37\n",
       "1                         temp  2.30\n",
       "6                     mnth_Jan  2.26\n",
       "5                     mnth_Feb  2.22\n",
       "0                           yr  2.03\n",
       "7                     mnth_Nov  1.77\n",
       "4                     mnth_Dec  1.51\n",
       "10      weathersit_Mist_Cloudy  1.50\n",
       "8                     mnth_Sep  1.17\n",
       "9   weathersit_Light_Snow_Rain  1.05"
      ]
     },
     "execution_count": 87,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Check for the VIF values of the feature variables. \n",
    "from statsmodels.stats.outliers_influence import variance_inflation_factor\n",
    "\n",
    "# Create a dataframe that will contain the names of all the feature variables and their respective VIFs\n",
    "vif = pd.DataFrame()\n",
    "vif['Features'] = X_train_new.columns\n",
    "vif['VIF'] = [variance_inflation_factor(X_train_new.values, i) for i in range(X_train_new.shape[1])]\n",
    "vif['VIF'] = round(vif['VIF'], 2)\n",
    "vif = vif.sort_values(by = \"VIF\", ascending = False)\n",
    "vif"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "* From the VIF calculation we could find that there is no multicollinearity existing between the predictor variables, as all the values are within permissible range of below 5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > MAKING PREDICTION USING FINAL MODEL"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now that we have fitted the model and checked the assumptions, it's time to go ahead and make predictions using the final model (lr5)\n",
    "\n",
    "<span style='background: lightGreen '> Applying the scaling on the test sets  </span>  <br> "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 88,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Apply scaler() to all numeric variables in test dataset. Note: we will only use scaler.transform, \n",
    "# as we want to use the metrics that the model learned from the training data to be applied on the test data. \n",
    "# In other words, we want to prevent the information leak from train to test dataset.\n",
    "\n",
    "num_vars = ['temp', 'atemp', 'hum', 'windspeed','cnt']\n",
    "\n",
    "df_test[num_vars] = scaler.transform(df_test[num_vars])"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 89,
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
       "      <th>yr</th>\n",
       "      <th>holiday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>cnt</th>\n",
       "      <th>season_Spring</th>\n",
       "      <th>season_Summer</th>\n",
       "      <th>...</th>\n",
       "      <th>mnth_Oct</th>\n",
       "      <th>mnth_Sep</th>\n",
       "      <th>weekday_1</th>\n",
       "      <th>weekday_2</th>\n",
       "      <th>weekday_3</th>\n",
       "      <th>weekday_4</th>\n",
       "      <th>weekday_5</th>\n",
       "      <th>weekday_6</th>\n",
       "      <th>weathersit_Light_Snow_Rain</th>\n",
       "      <th>weathersit_Mist_Cloudy</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>22</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.046591</td>\n",
       "      <td>0.025950</td>\n",
       "      <td>0.453529</td>\n",
       "      <td>0.462217</td>\n",
       "      <td>0.110907</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>468</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0.543115</td>\n",
       "      <td>0.536771</td>\n",
       "      <td>0.522511</td>\n",
       "      <td>0.347424</td>\n",
       "      <td>0.855729</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>553</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.951196</td>\n",
       "      <td>0.933712</td>\n",
       "      <td>0.596104</td>\n",
       "      <td>0.212829</td>\n",
       "      <td>0.534975</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>504</th>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.699909</td>\n",
       "      <td>0.662746</td>\n",
       "      <td>0.551083</td>\n",
       "      <td>0.478229</td>\n",
       "      <td>0.817648</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>353</th>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0.407087</td>\n",
       "      <td>0.416610</td>\n",
       "      <td>0.618615</td>\n",
       "      <td>0.080770</td>\n",
       "      <td>0.428900</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>...</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>5 rows Ã— 30 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "     yr  holiday  workingday      temp     atemp       hum  windspeed  \\\n",
       "22    0        0           1  0.046591  0.025950  0.453529   0.462217   \n",
       "468   1        0           0  0.543115  0.536771  0.522511   0.347424   \n",
       "553   1        0           1  0.951196  0.933712  0.596104   0.212829   \n",
       "504   1        0           1  0.699909  0.662746  0.551083   0.478229   \n",
       "353   0        0           1  0.407087  0.416610  0.618615   0.080770   \n",
       "\n",
       "          cnt  season_Spring  season_Summer  ...  mnth_Oct  mnth_Sep  \\\n",
       "22   0.110907              1              0  ...         0         0   \n",
       "468  0.855729              0              1  ...         0         0   \n",
       "553  0.534975              0              0  ...         0         0   \n",
       "504  0.817648              0              1  ...         0         0   \n",
       "353  0.428900              0              0  ...         0         0   \n",
       "\n",
       "     weekday_1  weekday_2  weekday_3  weekday_4  weekday_5  weekday_6  \\\n",
       "22           0          1          0          0          0          0   \n",
       "468          0          0          0          0          0          0   \n",
       "553          1          0          0          0          0          0   \n",
       "504          1          0          0          0          0          0   \n",
       "353          0          0          0          1          0          0   \n",
       "\n",
       "     weathersit_Light_Snow_Rain  weathersit_Mist_Cloudy  \n",
       "22                            0                       0  \n",
       "468                           0                       0  \n",
       "553                           0                       0  \n",
       "504                           0                       0  \n",
       "353                           0                       1  \n",
       "\n",
       "[5 rows x 30 columns]"
      ]
     },
     "execution_count": 89,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_test.head()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 90,
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
       "      <th>yr</th>\n",
       "      <th>holiday</th>\n",
       "      <th>workingday</th>\n",
       "      <th>temp</th>\n",
       "      <th>atemp</th>\n",
       "      <th>hum</th>\n",
       "      <th>windspeed</th>\n",
       "      <th>cnt</th>\n",
       "      <th>season_Spring</th>\n",
       "      <th>season_Summer</th>\n",
       "      <th>...</th>\n",
       "      <th>mnth_Oct</th>\n",
       "      <th>mnth_Sep</th>\n",
       "      <th>weekday_1</th>\n",
       "      <th>weekday_2</th>\n",
       "      <th>weekday_3</th>\n",
       "      <th>weekday_4</th>\n",
       "      <th>weekday_5</th>\n",
       "      <th>weekday_6</th>\n",
       "      <th>weathersit_Light_Snow_Rain</th>\n",
       "      <th>weathersit_Mist_Cloudy</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "      <td>219.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>0.493151</td>\n",
       "      <td>0.041096</td>\n",
       "      <td>0.689498</td>\n",
       "      <td>0.551225</td>\n",
       "      <td>0.527528</td>\n",
       "      <td>0.662567</td>\n",
       "      <td>0.346706</td>\n",
       "      <td>0.518889</td>\n",
       "      <td>0.237443</td>\n",
       "      <td>0.264840</td>\n",
       "      <td>...</td>\n",
       "      <td>0.086758</td>\n",
       "      <td>0.082192</td>\n",
       "      <td>0.146119</td>\n",
       "      <td>0.123288</td>\n",
       "      <td>0.168950</td>\n",
       "      <td>0.150685</td>\n",
       "      <td>0.132420</td>\n",
       "      <td>0.141553</td>\n",
       "      <td>0.036530</td>\n",
       "      <td>0.324201</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>0.501098</td>\n",
       "      <td>0.198967</td>\n",
       "      <td>0.463759</td>\n",
       "      <td>0.229463</td>\n",
       "      <td>0.215434</td>\n",
       "      <td>0.143562</td>\n",
       "      <td>0.159553</td>\n",
       "      <td>0.219953</td>\n",
       "      <td>0.426491</td>\n",
       "      <td>0.442259</td>\n",
       "      <td>...</td>\n",
       "      <td>0.282125</td>\n",
       "      <td>0.275286</td>\n",
       "      <td>0.354034</td>\n",
       "      <td>0.329520</td>\n",
       "      <td>0.375566</td>\n",
       "      <td>0.358561</td>\n",
       "      <td>0.339723</td>\n",
       "      <td>0.349389</td>\n",
       "      <td>0.188034</td>\n",
       "      <td>0.469148</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.046591</td>\n",
       "      <td>0.025950</td>\n",
       "      <td>0.301299</td>\n",
       "      <td>0.073090</td>\n",
       "      <td>0.055683</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.356479</td>\n",
       "      <td>0.348019</td>\n",
       "      <td>0.553031</td>\n",
       "      <td>0.232689</td>\n",
       "      <td>0.364703</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.557653</td>\n",
       "      <td>0.549198</td>\n",
       "      <td>0.662338</td>\n",
       "      <td>0.328208</td>\n",
       "      <td>0.525771</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.751309</td>\n",
       "      <td>0.709163</td>\n",
       "      <td>0.762338</td>\n",
       "      <td>0.435708</td>\n",
       "      <td>0.676887</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>0.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>0.984424</td>\n",
       "      <td>0.980934</td>\n",
       "      <td>1.010390</td>\n",
       "      <td>0.824380</td>\n",
       "      <td>0.963300</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>...</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "      <td>1.000000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>8 rows Ã— 30 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "               yr     holiday  workingday        temp       atemp         hum  \\\n",
       "count  219.000000  219.000000  219.000000  219.000000  219.000000  219.000000   \n",
       "mean     0.493151    0.041096    0.689498    0.551225    0.527528    0.662567   \n",
       "std      0.501098    0.198967    0.463759    0.229463    0.215434    0.143562   \n",
       "min      0.000000    0.000000    0.000000    0.046591    0.025950    0.301299   \n",
       "25%      0.000000    0.000000    0.000000    0.356479    0.348019    0.553031   \n",
       "50%      0.000000    0.000000    1.000000    0.557653    0.549198    0.662338   \n",
       "75%      1.000000    0.000000    1.000000    0.751309    0.709163    0.762338   \n",
       "max      1.000000    1.000000    1.000000    0.984424    0.980934    1.010390   \n",
       "\n",
       "        windspeed         cnt  season_Spring  season_Summer  ...    mnth_Oct  \\\n",
       "count  219.000000  219.000000     219.000000     219.000000  ...  219.000000   \n",
       "mean     0.346706    0.518889       0.237443       0.264840  ...    0.086758   \n",
       "std      0.159553    0.219953       0.426491       0.442259  ...    0.282125   \n",
       "min      0.073090    0.055683       0.000000       0.000000  ...    0.000000   \n",
       "25%      0.232689    0.364703       0.000000       0.000000  ...    0.000000   \n",
       "50%      0.328208    0.525771       0.000000       0.000000  ...    0.000000   \n",
       "75%      0.435708    0.676887       0.000000       1.000000  ...    0.000000   \n",
       "max      0.824380    0.963300       1.000000       1.000000  ...    1.000000   \n",
       "\n",
       "         mnth_Sep   weekday_1   weekday_2   weekday_3   weekday_4   weekday_5  \\\n",
       "count  219.000000  219.000000  219.000000  219.000000  219.000000  219.000000   \n",
       "mean     0.082192    0.146119    0.123288    0.168950    0.150685    0.132420   \n",
       "std      0.275286    0.354034    0.329520    0.375566    0.358561    0.339723   \n",
       "min      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "25%      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "50%      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "75%      0.000000    0.000000    0.000000    0.000000    0.000000    0.000000   \n",
       "max      1.000000    1.000000    1.000000    1.000000    1.000000    1.000000   \n",
       "\n",
       "        weekday_6  weathersit_Light_Snow_Rain  weathersit_Mist_Cloudy  \n",
       "count  219.000000                  219.000000              219.000000  \n",
       "mean     0.141553                    0.036530                0.324201  \n",
       "std      0.349389                    0.188034                0.469148  \n",
       "min      0.000000                    0.000000                0.000000  \n",
       "25%      0.000000                    0.000000                0.000000  \n",
       "50%      0.000000                    0.000000                0.000000  \n",
       "75%      0.000000                    0.000000                1.000000  \n",
       "max      1.000000                    1.000000                1.000000  \n",
       "\n",
       "[8 rows x 30 columns]"
      ]
     },
     "execution_count": 90,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df_test.describe()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<span style='background: lightGreen '>  Dividing into X_test and y_test </span>  <br>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 91,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Int64Index: 219 entries, 22 to 313\n",
      "Data columns (total 29 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   yr                          219 non-null    int64  \n",
      " 1   holiday                     219 non-null    int64  \n",
      " 2   workingday                  219 non-null    int64  \n",
      " 3   temp                        219 non-null    float64\n",
      " 4   atemp                       219 non-null    float64\n",
      " 5   hum                         219 non-null    float64\n",
      " 6   windspeed                   219 non-null    float64\n",
      " 7   season_Spring               219 non-null    uint8  \n",
      " 8   season_Summer               219 non-null    uint8  \n",
      " 9   season_Winter               219 non-null    uint8  \n",
      " 10  mnth_Aug                    219 non-null    uint8  \n",
      " 11  mnth_Dec                    219 non-null    uint8  \n",
      " 12  mnth_Feb                    219 non-null    uint8  \n",
      " 13  mnth_Jan                    219 non-null    uint8  \n",
      " 14  mnth_Jul                    219 non-null    uint8  \n",
      " 15  mnth_Jun                    219 non-null    uint8  \n",
      " 16  mnth_Mar                    219 non-null    uint8  \n",
      " 17  mnth_May                    219 non-null    uint8  \n",
      " 18  mnth_Nov                    219 non-null    uint8  \n",
      " 19  mnth_Oct                    219 non-null    uint8  \n",
      " 20  mnth_Sep                    219 non-null    uint8  \n",
      " 21  weekday_1                   219 non-null    uint8  \n",
      " 22  weekday_2                   219 non-null    uint8  \n",
      " 23  weekday_3                   219 non-null    uint8  \n",
      " 24  weekday_4                   219 non-null    uint8  \n",
      " 25  weekday_5                   219 non-null    uint8  \n",
      " 26  weekday_6                   219 non-null    uint8  \n",
      " 27  weathersit_Light_Snow_Rain  219 non-null    uint8  \n",
      " 28  weathersit_Mist_Cloudy      219 non-null    uint8  \n",
      "dtypes: float64(4), int64(3), uint8(22)\n",
      "memory usage: 18.4 KB\n"
     ]
    }
   ],
   "source": [
    "y_test = df_test.pop('cnt')\n",
    "X_test = df_test\n",
    "X_test.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 92,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "<class 'pandas.core.frame.DataFrame'>\n",
      "Int64Index: 219 entries, 22 to 313\n",
      "Data columns (total 12 columns):\n",
      " #   Column                      Non-Null Count  Dtype  \n",
      "---  ------                      --------------  -----  \n",
      " 0   const                       219 non-null    float64\n",
      " 1   yr                          219 non-null    int64  \n",
      " 2   temp                        219 non-null    float64\n",
      " 3   season_Spring               219 non-null    uint8  \n",
      " 4   season_Winter               219 non-null    uint8  \n",
      " 5   mnth_Dec                    219 non-null    uint8  \n",
      " 6   mnth_Feb                    219 non-null    uint8  \n",
      " 7   mnth_Jan                    219 non-null    uint8  \n",
      " 8   mnth_Nov                    219 non-null    uint8  \n",
      " 9   mnth_Sep                    219 non-null    uint8  \n",
      " 10  weathersit_Light_Snow_Rain  219 non-null    uint8  \n",
      " 11  weathersit_Mist_Cloudy      219 non-null    uint8  \n",
      "dtypes: float64(2), int64(1), uint8(9)\n",
      "memory usage: 8.8 KB\n"
     ]
    }
   ],
   "source": [
    "#Selecting the variables that were part of final model.\n",
    "col1=X_train_new.columns\n",
    "X_test=X_test[col1]\n",
    "# Adding constant variable to test dataframe\n",
    "X_test_lm5 = sm.add_constant(X_test)\n",
    "X_test_lm5.info()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 93,
   "metadata": {},
   "outputs": [],
   "source": [
    "# Making predictions using the final model (lr6)\n",
    "\n",
    "y_pred = lr5.predict(X_test_lm5)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > MODEL EVALUATION"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 94,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
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
    "# Plotting y_test and y_pred to understand the spread\n",
    "\n",
    "fig = plt.figure()\n",
    "plt.scatter(y_test, y_pred, alpha=.5)\n",
    "fig.suptitle('y_test vs y_pred', fontsize = 20)              # Plot heading \n",
    "plt.xlabel('y_test', fontsize = 18)                          # X-label\n",
    "plt.ylabel('y_pred', fontsize = 16) \n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## R^2 Value for TEST"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 95,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.8224454904426144"
      ]
     },
     "execution_count": 95,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "from sklearn.metrics import r2_score\n",
    "r2_score(y_test, y_pred)\n",
    "r2_score(y_test, y_pred)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Adjusted R^2 Value for TEST"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 96,
   "metadata": {},
   "outputs": [],
   "source": [
    "# We already have the value of R^2 (calculated in above step)\n",
    "\n",
    "r2=0.8224454904426144"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 97,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "(219, 11)"
      ]
     },
     "execution_count": 97,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Get the shape of X_test\n",
    "X_test.shape"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 98,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.8130102266497099"
      ]
     },
     "execution_count": 98,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# n is number of rows in X\n",
    "\n",
    "n = X_test.shape[0]\n",
    "\n",
    "\n",
    "# Number of features (predictors, p) is the shape along axis 1\n",
    "p = X_test.shape[1]\n",
    "\n",
    "# We find the Adjusted R-squared using the formula\n",
    "\n",
    "adjusted_r2 = 1-(1-r2)*(n-1)/(n-p-1)\n",
    "adjusted_r2"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Red' > Final Result Comparison"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "* Train R^2 :0.817\n",
    "* Train Adjusted R^2 :0.813\n",
    "* Test R^2 :0.822\n",
    "* Test Adjusted R^2 :0.813\n",
    "* This seems to be a really good model that can very well 'Generalize' various datasets."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 99,
   "metadata": {},
   "outputs": [],
   "source": [
    "r2_train=0.817\n",
    "r2_test=0.822"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 100,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Difference in r2 Score(%) 0.5000000000000004\n"
     ]
    }
   ],
   "source": [
    "# Checking the difference between the test-train r2 score \n",
    "print('Difference in r2 Score(%)',(-r2_train + r2_test)*100)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 101,
   "metadata": {},
   "outputs": [],
   "source": [
    "Train_Adjusted_R2 = 0.813\n",
    "Test_Adjusted_R2 = 0.813"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 102,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Difference in Adjusted_R2 Score(%) 0.0\n"
     ]
    }
   ],
   "source": [
    "# Checking the difference between the test-train Adjusted_R2 score \n",
    "print('Difference in Adjusted_R2 Score(%)',(Train_Adjusted_R2-Test_Adjusted_R2)*100)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# <span style = 'color : Green' > **FINAL REPORT**"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "\n",
    "As per our final Model, the top 3 predictor variables that influences the bike booking are:\n",
    "\n",
    "* Temperature (temp) - A coefficient value of â€˜0.375922â€™ indicated that a unit increase in temp variable increases the bike hire numbers by 0.375922 units. <br>\n",
    "\n",
    "* Weather Situation 3 (weathersit_3)(Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered) - A coefficient value of â€˜-0.333164â€™ indicated that, w.r.t Weathersit_3, a unit increase in Weathersit_3 variable decreases the bike hire numbers by 0.333164 units.<br>\n",
    "\n",
    "* Year (yr) - A coefficient value of â€˜0.232965â€™ indicated that a unit increase in yr variable increases the bike hire numbers by 0.232965 units.<br>\n",
    "\n",
    "\n",
    "\n",
    "So, it's suggested to consider these variables utmost importance while planning, to achieve maximum Booking<br>"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
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
   "version": "3.8.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}