Github hyperlink: https://adejareadesokan.github.io/nba-statistics-project/

Ayomide Banjo-Obaleye and Adejare Adesokan



Project title: Determining the efficiency of a professional basketball player

Project description: The box score does not show the complete performance of a professional basketball player, it shows only a fraction and that is what sports fans tend to judge a player based on. Our project "Determining the efficiency of a professional basketball player" will explore the relationship that exists between scoring volume and shooting efficiency of a professional player

Using regular and advanced statistics from  the 2025-2026 NBA regular season, we will investigate how points per game, field-goal percentage, three-point attempts, and effective field-goal percentage relate to one another. The main goal is to determine whether players who score more points per game are also more efficient shooters, and whether three-point shooting is associated with overall scoring efficiency. We will also investigate correlations between scoring volume and shooting efficiency to identify patterns in offensive performance.

The findings may help explain why evaluating players based solely on points per game can be misleading. By examining both scoring production and efficiency, this project aims to provide a more informative perspective on individual offensive performance that could be useful to basketball fans, coaches, and analysts.

The goals of this project are to:
1. Collect and clean data: Obtain NBA player statistics, organize the data into a structured format, and handle missing values and duplicate player records.
2. Analyze scoring efficiency: Investigate the relationship between points per game, field-goal percentage, three-point attempts, and effective field-goal percentage.
3. Explore player performance:  Calculate descriptive statistics and identify the league's highest-scoring players and most efficient shooters.
4. Visualize statistical relationships:Create graphs and charts to identify trends and compare scoring volume with shooting efficiency.
5. generate meaningful insight: Determine whether high scoring necessarily corresponds to high shooting efficiency and explain what these patterns reveal about offensive performance.

Research questions
1. Which players score the most points per game?
2. Are players who shoot more three pointers also more efficient scorers
3. Is scoring volume related to scoring efficiency
4. Do high-scoring players necessarily shoot efficiently?
5. What is the difference in efficiency by position

Links to the data: 2 datasets
https://www.basketball-reference.com/leagues/NBA_2026_per_game.html
https://www.basketball-reference.com/leagues/NBA_2026_advanced.html

Moving forward:

Collaboration plan: Jare and I are roommates so we intend to prioritize collective effort and dedication, we plan to work on it at least every other day and more if need be aside from breaks and traveling that would require scheduled zoom meetings. we will mainly be working on this project in person about 1-2 times a week using a shared google colab and pushing our changes to a shared GitHub repository. Communication would come easy to us as we are roommates but we intend to document everything including ideas and changes to be made. We intend to hold eachother accountable and ensure we are on track to complete every milestone on time.

We will start by loading in the datasets from github


```python
import pandas as pd
url = "https://raw.githubusercontent.com/adejareadesokan/nba-statistics-project/refs/heads/main/data/raw/per_game.csv"
df = pd.read_csv(url) #loading in regular season dataset
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 733 entries, 0 to 732
    Data columns (total 32 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Rk                 733 non-null    int64  
     1   Player             733 non-null    object 
     2   Age                733 non-null    int64  
     3   Team               733 non-null    object 
     4   Pos                733 non-null    object 
     5   G                  733 non-null    int64  
     6   GS                 733 non-null    int64  
     7   MP                 733 non-null    float64
     8   FG                 733 non-null    float64
     9   FGA                733 non-null    float64
     10  FG%                730 non-null    float64
     11  3P                 733 non-null    float64
     12  3PA                733 non-null    float64
     13  3P%                682 non-null    float64
     14  2P                 733 non-null    float64
     15  2PA                733 non-null    float64
     16  2P%                726 non-null    float64
     17  eFG%               730 non-null    float64
     18  FT                 733 non-null    float64
     19  FTA                733 non-null    float64
     20  FT%                702 non-null    float64
     21  ORB                733 non-null    float64
     22  DRB                733 non-null    float64
     23  TRB                733 non-null    float64
     24  AST                733 non-null    float64
     25  STL                733 non-null    float64
     26  BLK                733 non-null    float64
     27  TOV                733 non-null    float64
     28  PF                 733 non-null    float64
     29  PTS                733 non-null    float64
     30  Awards             54 non-null     object 
     31  Player-additional  733 non-null    object 
    dtypes: float64(23), int64(4), object(5)
    memory usage: 183.4+ KB



```python
adv_url = "https://raw.githubusercontent.com/adejareadesokan/nba-statistics-project/refs/heads/main/data/raw/advanced.csv"
adv_df = pd.read_csv(adv_url)#loading in advanced stats datasets
adv_df.head()
```





  <div id="df-1dbf750d-a63f-491e-80c4-cdadbba04d39" class="colab-df-container">
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
      <th>Rk</th>
      <th>Player</th>
      <th>Age</th>
      <th>Team</th>
      <th>Pos</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>...</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>Awards</th>
      <th>Player-additional</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Amen Thompson</td>
      <td>23</td>
      <td>HOU</td>
      <td>PG</td>
      <td>79</td>
      <td>79</td>
      <td>2953</td>
      <td>18.9</td>
      <td>0.594</td>
      <td>...</td>
      <td>6.5</td>
      <td>3.8</td>
      <td>10.3</td>
      <td>0.167</td>
      <td>1.6</td>
      <td>1.0</td>
      <td>2.6</td>
      <td>3.4</td>
      <td>DPOY-8</td>
      <td>thompam01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Kevin Durant</td>
      <td>37</td>
      <td>HOU</td>
      <td>SF</td>
      <td>78</td>
      <td>78</td>
      <td>2840</td>
      <td>21.1</td>
      <td>0.641</td>
      <td>...</td>
      <td>7.5</td>
      <td>3.2</td>
      <td>10.7</td>
      <td>0.180</td>
      <td>4.4</td>
      <td>0.1</td>
      <td>4.5</td>
      <td>4.7</td>
      <td>CPOY-9ASNBA2</td>
      <td>duranke01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Desmond Bane</td>
      <td>27</td>
      <td>ORL</td>
      <td>SG</td>
      <td>82</td>
      <td>82</td>
      <td>2756</td>
      <td>16.9</td>
      <td>0.607</td>
      <td>...</td>
      <td>4.8</td>
      <td>2.6</td>
      <td>7.4</td>
      <td>0.129</td>
      <td>1.6</td>
      <td>-0.2</td>
      <td>1.4</td>
      <td>2.3</td>
      <td>NaN</td>
      <td>banede01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Toumani Camara</td>
      <td>25</td>
      <td>POR</td>
      <td>PF</td>
      <td>82</td>
      <td>82</td>
      <td>2731</td>
      <td>11.3</td>
      <td>0.578</td>
      <td>...</td>
      <td>2.1</td>
      <td>2.7</td>
      <td>4.8</td>
      <td>0.084</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>-1.0</td>
      <td>0.7</td>
      <td>NaN</td>
      <td>camarto01</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jabari Smith Jr.</td>
      <td>22</td>
      <td>HOU</td>
      <td>PF</td>
      <td>77</td>
      <td>77</td>
      <td>2705</td>
      <td>13.5</td>
      <td>0.570</td>
      <td>...</td>
      <td>3.4</td>
      <td>3.3</td>
      <td>6.6</td>
      <td>0.118</td>
      <td>-0.5</td>
      <td>-0.2</td>
      <td>-0.7</td>
      <td>0.9</td>
      <td>NaN</td>
      <td>smithja05</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-1dbf750d-a63f-491e-80c4-cdadbba04d39')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-1dbf750d-a63f-491e-80c4-cdadbba04d39 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-1dbf750d-a63f-491e-80c4-cdadbba04d39');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>

    </div>
  </div>




We will tidy up the regular dataset first. We will first analyze the dataset.


```python
df.info() # listing out the metadata of the dataset
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 733 entries, 0 to 732
    Data columns (total 32 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Rk                 733 non-null    int64  
     1   Player             733 non-null    object 
     2   Age                733 non-null    int64  
     3   Team               733 non-null    object 
     4   Pos                733 non-null    object 
     5   G                  733 non-null    int64  
     6   GS                 733 non-null    int64  
     7   MP                 733 non-null    float64
     8   FG                 733 non-null    float64
     9   FGA                733 non-null    float64
     10  FG%                730 non-null    float64
     11  3P                 733 non-null    float64
     12  3PA                733 non-null    float64
     13  3P%                682 non-null    float64
     14  2P                 733 non-null    float64
     15  2PA                733 non-null    float64
     16  2P%                726 non-null    float64
     17  eFG%               730 non-null    float64
     18  FT                 733 non-null    float64
     19  FTA                733 non-null    float64
     20  FT%                702 non-null    float64
     21  ORB                733 non-null    float64
     22  DRB                733 non-null    float64
     23  TRB                733 non-null    float64
     24  AST                733 non-null    float64
     25  STL                733 non-null    float64
     26  BLK                733 non-null    float64
     27  TOV                733 non-null    float64
     28  PF                 733 non-null    float64
     29  PTS                733 non-null    float64
     30  Awards             54 non-null     object 
     31  Player-additional  733 non-null    object 
    dtypes: float64(23), int64(4), object(5)
    memory usage: 183.4+ KB



```python
df #displaying the structure of the data frame
```





  <div id="df-fb5ed223-c837-4f42-85f4-83b4098768e6" class="colab-df-container">
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
      <th>Rk</th>
      <th>Player</th>
      <th>Age</th>
      <th>Team</th>
      <th>Pos</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>FG</th>
      <th>FGA</th>
      <th>...</th>
      <th>DRB</th>
      <th>TRB</th>
      <th>AST</th>
      <th>STL</th>
      <th>BLK</th>
      <th>TOV</th>
      <th>PF</th>
      <th>PTS</th>
      <th>Awards</th>
      <th>Player-additional</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Luka Dončić</td>
      <td>26</td>
      <td>LAL</td>
      <td>PG</td>
      <td>64</td>
      <td>64</td>
      <td>35.8</td>
      <td>10.8</td>
      <td>22.8</td>
      <td>...</td>
      <td>7.1</td>
      <td>7.7</td>
      <td>8.3</td>
      <td>1.6</td>
      <td>0.5</td>
      <td>4.0</td>
      <td>2.4</td>
      <td>33.5</td>
      <td>MVP-4CPOY-8ASNBA1</td>
      <td>doncilu01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Shai Gilgeous-Alexander</td>
      <td>27</td>
      <td>OKC</td>
      <td>PG</td>
      <td>68</td>
      <td>68</td>
      <td>33.2</td>
      <td>10.8</td>
      <td>19.4</td>
      <td>...</td>
      <td>3.7</td>
      <td>4.3</td>
      <td>6.6</td>
      <td>1.4</td>
      <td>0.8</td>
      <td>2.2</td>
      <td>2.0</td>
      <td>31.1</td>
      <td>MVP-1CPOY-1ASNBA1</td>
      <td>gilgesh01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Anthony Edwards</td>
      <td>24</td>
      <td>MIN</td>
      <td>SG</td>
      <td>61</td>
      <td>60</td>
      <td>35.0</td>
      <td>9.9</td>
      <td>20.2</td>
      <td>...</td>
      <td>4.4</td>
      <td>5.0</td>
      <td>3.7</td>
      <td>1.4</td>
      <td>0.8</td>
      <td>2.9</td>
      <td>1.9</td>
      <td>28.8</td>
      <td>CPOY-3AS</td>
      <td>edwaran01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Jaylen Brown</td>
      <td>29</td>
      <td>BOS</td>
      <td>SF</td>
      <td>71</td>
      <td>71</td>
      <td>34.4</td>
      <td>10.4</td>
      <td>21.7</td>
      <td>...</td>
      <td>5.8</td>
      <td>6.9</td>
      <td>5.1</td>
      <td>1.0</td>
      <td>0.4</td>
      <td>3.6</td>
      <td>2.7</td>
      <td>28.7</td>
      <td>MVP-6ASNBA2</td>
      <td>brownja02</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Tyrese Maxey</td>
      <td>25</td>
      <td>PHI</td>
      <td>PG</td>
      <td>70</td>
      <td>70</td>
      <td>38.0</td>
      <td>9.9</td>
      <td>21.4</td>
      <td>...</td>
      <td>3.8</td>
      <td>4.1</td>
      <td>6.6</td>
      <td>1.9</td>
      <td>0.8</td>
      <td>2.4</td>
      <td>2.2</td>
      <td>28.3</td>
      <td>CPOY-7ASNBA3</td>
      <td>maxeyty01</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>728</th>
      <td>578</td>
      <td>Jahmyl Telfort</td>
      <td>24</td>
      <td>LAC</td>
      <td>PF</td>
      <td>8</td>
      <td>0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>...</td>
      <td>0.4</td>
      <td>0.4</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.0</td>
      <td>0.1</td>
      <td>0.6</td>
      <td>0.1</td>
      <td>NaN</td>
      <td>telfoja01</td>
    </tr>
    <tr>
      <th>729</th>
      <td>579</td>
      <td>Darius Brown II</td>
      <td>26</td>
      <td>CLE</td>
      <td>SG</td>
      <td>1</td>
      <td>0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>brownda04</td>
    </tr>
    <tr>
      <th>730</th>
      <td>580</td>
      <td>Noa Essengue</td>
      <td>19</td>
      <td>CHI</td>
      <td>PF</td>
      <td>2</td>
      <td>0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>1.5</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>essenno01</td>
    </tr>
    <tr>
      <th>731</th>
      <td>581</td>
      <td>Tosan Evbuomwan</td>
      <td>24</td>
      <td>NYK</td>
      <td>SF</td>
      <td>5</td>
      <td>0</td>
      <td>1.6</td>
      <td>0.0</td>
      <td>0.2</td>
      <td>...</td>
      <td>0.2</td>
      <td>0.4</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.2</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>evbuoto01</td>
    </tr>
    <tr>
      <th>732</th>
      <td>582</td>
      <td>Chucky Hepburn</td>
      <td>22</td>
      <td>TOR</td>
      <td>PG</td>
      <td>2</td>
      <td>0</td>
      <td>6.5</td>
      <td>0.0</td>
      <td>3.0</td>
      <td>...</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>1.0</td>
      <td>0.5</td>
      <td>0.0</td>
      <td>0.5</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>hepbuch01</td>
    </tr>
  </tbody>
</table>
<p>733 rows × 32 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-fb5ed223-c837-4f42-85f4-83b4098768e6')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-fb5ed223-c837-4f42-85f4-83b4098768e6 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-fb5ed223-c837-4f42-85f4-83b4098768e6');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>

    </div>
  </div>




Since players can play for multiple teams, we would want to aggregate the rows, but our source already combined them on the first record, so we would drop all duplicates other than the first record.


```python
df = df.drop_duplicates(subset = "Player", keep = "first") # removing all duplicate instance of players
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 582 entries, 0 to 732
    Data columns (total 32 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Rk                 582 non-null    int64  
     1   Player             582 non-null    object 
     2   Age                582 non-null    int64  
     3   Team               582 non-null    object 
     4   Pos                582 non-null    object 
     5   G                  582 non-null    int64  
     6   GS                 582 non-null    int64  
     7   MP                 582 non-null    float64
     8   FG                 582 non-null    float64
     9   FGA                582 non-null    float64
     10  FG%                582 non-null    float64
     11  3P                 582 non-null    float64
     12  3PA                582 non-null    float64
     13  3P%                547 non-null    float64
     14  2P                 582 non-null    float64
     15  2PA                582 non-null    float64
     16  2P%                579 non-null    float64
     17  eFG%               582 non-null    float64
     18  FT                 582 non-null    float64
     19  FTA                582 non-null    float64
     20  FT%                563 non-null    float64
     21  ORB                582 non-null    float64
     22  DRB                582 non-null    float64
     23  TRB                582 non-null    float64
     24  AST                582 non-null    float64
     25  STL                582 non-null    float64
     26  BLK                582 non-null    float64
     27  TOV                582 non-null    float64
     28  PF                 582 non-null    float64
     29  PTS                582 non-null    float64
     30  Awards             54 non-null     object 
     31  Player-additional  582 non-null    object 
    dtypes: float64(23), int64(4), object(5)
    memory usage: 150.0+ KB


The no of entries reduced from 733 to 582 which is equal to the number of players who played in the NBA in the 25-26 season

We also drop `Rk` as we would not be making use of it


```python
df = df.drop( columns= ["Rk"]) # dropping Rk as we won't be using it
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 582 entries, 0 to 732
    Data columns (total 31 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Player             582 non-null    object 
     1   Age                582 non-null    int64  
     2   Team               582 non-null    object 
     3   Pos                582 non-null    object 
     4   G                  582 non-null    int64  
     5   GS                 582 non-null    int64  
     6   MP                 582 non-null    float64
     7   FG                 582 non-null    float64
     8   FGA                582 non-null    float64
     9   FG%                582 non-null    float64
     10  3P                 582 non-null    float64
     11  3PA                582 non-null    float64
     12  3P%                547 non-null    float64
     13  2P                 582 non-null    float64
     14  2PA                582 non-null    float64
     15  2P%                579 non-null    float64
     16  eFG%               582 non-null    float64
     17  FT                 582 non-null    float64
     18  FTA                582 non-null    float64
     19  FT%                563 non-null    float64
     20  ORB                582 non-null    float64
     21  DRB                582 non-null    float64
     22  TRB                582 non-null    float64
     23  AST                582 non-null    float64
     24  STL                582 non-null    float64
     25  BLK                582 non-null    float64
     26  TOV                582 non-null    float64
     27  PF                 582 non-null    float64
     28  PTS                582 non-null    float64
     29  Awards             54 non-null     object 
     30  Player-additional  582 non-null    object 
    dtypes: float64(23), int64(3), object(5)
    memory usage: 145.5+ KB


We would also have to change the column variables to a python and pandas compatible format. symbols such as percentages and dashes are reserved in python. Additionally, Python does not allow for variables to sart with a number, hence we have to change that.


```python
df.columns = df.columns.str.lower().str.replace("%","_pct").str.replace("-","_") # broadcasting python compatible column format
df = df.rename(columns={"3p":"fg3","3pa":"fg3a","3p_pct":"fg3_pct","2p":"fg2","2pa":"fg2a","2p_pct":"fg2_pct"}) #renaming columns to match python variable syntax

```


```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 582 entries, 0 to 732
    Data columns (total 31 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   player             582 non-null    object 
     1   age                582 non-null    int64  
     2   team               582 non-null    object 
     3   pos                582 non-null    object 
     4   g                  582 non-null    int64  
     5   gs                 582 non-null    int64  
     6   mp                 582 non-null    float64
     7   fg                 582 non-null    float64
     8   fga                582 non-null    float64
     9   fg_pct             582 non-null    float64
     10  fg3                582 non-null    float64
     11  fg3a               582 non-null    float64
     12  fg3_pct            547 non-null    float64
     13  fg2                582 non-null    float64
     14  fg2a               582 non-null    float64
     15  fg2_pct            579 non-null    float64
     16  efg_pct            582 non-null    float64
     17  ft                 582 non-null    float64
     18  fta                582 non-null    float64
     19  ft_pct             563 non-null    float64
     20  orb                582 non-null    float64
     21  drb                582 non-null    float64
     22  trb                582 non-null    float64
     23  ast                582 non-null    float64
     24  stl                582 non-null    float64
     25  blk                582 non-null    float64
     26  tov                582 non-null    float64
     27  pf                 582 non-null    float64
     28  pts                582 non-null    float64
     29  awards             54 non-null     object 
     30  player_additional  582 non-null    object 
    dtypes: float64(23), int64(3), object(5)
    memory usage: 145.5+ KB


The indexes were still set for 733 entries even though we filtered down to 582. We would have to reset the indices.


```python
df =df.reset_index(drop = True) #re-indexing data to natural number order
df.head()

```





  <div id="df-e2b085a1-25cc-481a-bf4c-cb10f0bfde6e" class="colab-df-container">
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
      <th>player</th>
      <th>age</th>
      <th>team</th>
      <th>pos</th>
      <th>g</th>
      <th>gs</th>
      <th>mp</th>
      <th>fg</th>
      <th>fga</th>
      <th>fg_pct</th>
      <th>...</th>
      <th>drb</th>
      <th>trb</th>
      <th>ast</th>
      <th>stl</th>
      <th>blk</th>
      <th>tov</th>
      <th>pf</th>
      <th>pts</th>
      <th>awards</th>
      <th>player_additional</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Luka Dončić</td>
      <td>26</td>
      <td>LAL</td>
      <td>PG</td>
      <td>64</td>
      <td>64</td>
      <td>35.8</td>
      <td>10.8</td>
      <td>22.8</td>
      <td>0.476</td>
      <td>...</td>
      <td>7.1</td>
      <td>7.7</td>
      <td>8.3</td>
      <td>1.6</td>
      <td>0.5</td>
      <td>4.0</td>
      <td>2.4</td>
      <td>33.5</td>
      <td>MVP-4CPOY-8ASNBA1</td>
      <td>doncilu01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Shai Gilgeous-Alexander</td>
      <td>27</td>
      <td>OKC</td>
      <td>PG</td>
      <td>68</td>
      <td>68</td>
      <td>33.2</td>
      <td>10.8</td>
      <td>19.4</td>
      <td>0.553</td>
      <td>...</td>
      <td>3.7</td>
      <td>4.3</td>
      <td>6.6</td>
      <td>1.4</td>
      <td>0.8</td>
      <td>2.2</td>
      <td>2.0</td>
      <td>31.1</td>
      <td>MVP-1CPOY-1ASNBA1</td>
      <td>gilgesh01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anthony Edwards</td>
      <td>24</td>
      <td>MIN</td>
      <td>SG</td>
      <td>61</td>
      <td>60</td>
      <td>35.0</td>
      <td>9.9</td>
      <td>20.2</td>
      <td>0.489</td>
      <td>...</td>
      <td>4.4</td>
      <td>5.0</td>
      <td>3.7</td>
      <td>1.4</td>
      <td>0.8</td>
      <td>2.9</td>
      <td>1.9</td>
      <td>28.8</td>
      <td>CPOY-3AS</td>
      <td>edwaran01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Jaylen Brown</td>
      <td>29</td>
      <td>BOS</td>
      <td>SF</td>
      <td>71</td>
      <td>71</td>
      <td>34.4</td>
      <td>10.4</td>
      <td>21.7</td>
      <td>0.477</td>
      <td>...</td>
      <td>5.8</td>
      <td>6.9</td>
      <td>5.1</td>
      <td>1.0</td>
      <td>0.4</td>
      <td>3.6</td>
      <td>2.7</td>
      <td>28.7</td>
      <td>MVP-6ASNBA2</td>
      <td>brownja02</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Tyrese Maxey</td>
      <td>25</td>
      <td>PHI</td>
      <td>PG</td>
      <td>70</td>
      <td>70</td>
      <td>38.0</td>
      <td>9.9</td>
      <td>21.4</td>
      <td>0.462</td>
      <td>...</td>
      <td>3.8</td>
      <td>4.1</td>
      <td>6.6</td>
      <td>1.9</td>
      <td>0.8</td>
      <td>2.4</td>
      <td>2.2</td>
      <td>28.3</td>
      <td>CPOY-7ASNBA3</td>
      <td>maxeyty01</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-e2b085a1-25cc-481a-bf4c-cb10f0bfde6e')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-e2b085a1-25cc-481a-bf4c-cb10f0bfde6e button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-e2b085a1-25cc-481a-bf4c-cb10f0bfde6e');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>

    </div>
  </div>




  Our awards column seems to be very untidy with multiple variables. We will clean it up.


```python
#cleaning up untidy awards column
df["mvp_rank"] = df["awards"].str.extract(r"MVP-(\d+)").astype(float)
df["all_nba"] = df["awards"].str.contains("NBA", na= False)
df["all_star"] = df["awards"].str.contains("AS", na= False)
df["cpoy_rank"] = df["awards"].str.extract(r"CPOY-(\d+)").astype(float)
df["dpoy_rank"] = df["awards"].str.extract(r"DPOY-(\d+)").astype(float)
df["mip_rank"] = df["awards"].str.extract(r"MIP-(\d+)").astype(float)
df["roy_rank"] = df["awards"].str.extract(r"ROY-(\d+?)(?=6MOY|[A-Z]|$)").astype(float)
df["sixmoy_rank"] = df["awards"].str.extract(r"6MOY-(\d+)").astype(float)
```

Now that we have cleaned it up, we can drop the original awards column.


```python
df =df.drop(columns=["awards"]) #dropping untidy awards column
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 582 entries, 0 to 581
    Data columns (total 38 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   player             582 non-null    object 
     1   age                582 non-null    int64  
     2   team               582 non-null    object 
     3   pos                582 non-null    object 
     4   g                  582 non-null    int64  
     5   gs                 582 non-null    int64  
     6   mp                 582 non-null    float64
     7   fg                 582 non-null    float64
     8   fga                582 non-null    float64
     9   fg_pct             582 non-null    float64
     10  fg3                582 non-null    float64
     11  fg3a               582 non-null    float64
     12  fg3_pct            547 non-null    float64
     13  fg2                582 non-null    float64
     14  fg2a               582 non-null    float64
     15  fg2_pct            579 non-null    float64
     16  efg_pct            582 non-null    float64
     17  ft                 582 non-null    float64
     18  fta                582 non-null    float64
     19  ft_pct             563 non-null    float64
     20  orb                582 non-null    float64
     21  drb                582 non-null    float64
     22  trb                582 non-null    float64
     23  ast                582 non-null    float64
     24  stl                582 non-null    float64
     25  blk                582 non-null    float64
     26  tov                582 non-null    float64
     27  pf                 582 non-null    float64
     28  pts                582 non-null    float64
     29  player_additional  582 non-null    object 
     30  mvp_rank           8 non-null      float64
     31  all_nba            582 non-null    bool   
     32  all_star           582 non-null    bool   
     33  cpoy_rank          10 non-null     float64
     34  dpoy_rank          13 non-null     float64
     35  mip_rank           1 non-null      float64
     36  roy_rank           5 non-null      float64
     37  sixmoy_rank        12 non-null     float64
    dtypes: bool(2), float64(29), int64(3), object(4)
    memory usage: 165.0+ KB


Players with fewer games and minutes played could skew our data. For example, a player could play only 6 games and played an average of 3 minutes per game, taking a few shots and making most of them, drastically increasing the values of our calculations. It is important they are filtered out, so we cut them off at those who have at least 20 games and average at least 15 minutes a game


```python
df = df[(df["g"] >= 20) & (df["mp"]>=15)].reset_index(drop = True) # filtering out players with little playtime
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 350 entries, 0 to 349
    Data columns (total 38 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   player             350 non-null    object 
     1   age                350 non-null    int64  
     2   team               350 non-null    object 
     3   pos                350 non-null    object 
     4   g                  350 non-null    int64  
     5   gs                 350 non-null    int64  
     6   mp                 350 non-null    float64
     7   fg                 350 non-null    float64
     8   fga                350 non-null    float64
     9   fg_pct             350 non-null    float64
     10  fg3                350 non-null    float64
     11  fg3a               350 non-null    float64
     12  fg3_pct            340 non-null    float64
     13  fg2                350 non-null    float64
     14  fg2a               350 non-null    float64
     15  fg2_pct            350 non-null    float64
     16  efg_pct            350 non-null    float64
     17  ft                 350 non-null    float64
     18  fta                350 non-null    float64
     19  ft_pct             350 non-null    float64
     20  orb                350 non-null    float64
     21  drb                350 non-null    float64
     22  trb                350 non-null    float64
     23  ast                350 non-null    float64
     24  stl                350 non-null    float64
     25  blk                350 non-null    float64
     26  tov                350 non-null    float64
     27  pf                 350 non-null    float64
     28  pts                350 non-null    float64
     29  player_additional  350 non-null    object 
     30  mvp_rank           8 non-null      float64
     31  all_nba            350 non-null    bool   
     32  all_star           350 non-null    bool   
     33  cpoy_rank          10 non-null     float64
     34  dpoy_rank          13 non-null     float64
     35  mip_rank           1 non-null      float64
     36  roy_rank           5 non-null      float64
     37  sixmoy_rank        12 non-null     float64
    dtypes: bool(2), float64(29), int64(3), object(4)
    memory usage: 99.2+ KB


If we then check the NaNs, we notice that the only columns with NaNs are 3-point field goal percentage, and the individual awards, the reason this is so is because they either got no votes as in individual awards  or they did not take any three point shots. It would not make sense to put 0 percent for the people who took no shots because there will be no way to tell them apart from those who took shots and didn't make any


```python
df.isna().sum()
```




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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>player</th>
      <td>0</td>
    </tr>
    <tr>
      <th>age</th>
      <td>0</td>
    </tr>
    <tr>
      <th>team</th>
      <td>0</td>
    </tr>
    <tr>
      <th>pos</th>
      <td>0</td>
    </tr>
    <tr>
      <th>g</th>
      <td>0</td>
    </tr>
    <tr>
      <th>gs</th>
      <td>0</td>
    </tr>
    <tr>
      <th>mp</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fga</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg3</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg3a</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg3_pct</th>
      <td>10</td>
    </tr>
    <tr>
      <th>fg2</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg2a</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg2_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>efg_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ft</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fta</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ft_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>orb</th>
      <td>0</td>
    </tr>
    <tr>
      <th>drb</th>
      <td>0</td>
    </tr>
    <tr>
      <th>trb</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ast</th>
      <td>0</td>
    </tr>
    <tr>
      <th>stl</th>
      <td>0</td>
    </tr>
    <tr>
      <th>blk</th>
      <td>0</td>
    </tr>
    <tr>
      <th>tov</th>
      <td>0</td>
    </tr>
    <tr>
      <th>pf</th>
      <td>0</td>
    </tr>
    <tr>
      <th>pts</th>
      <td>0</td>
    </tr>
    <tr>
      <th>player_additional</th>
      <td>0</td>
    </tr>
    <tr>
      <th>mvp_rank</th>
      <td>342</td>
    </tr>
    <tr>
      <th>all_nba</th>
      <td>0</td>
    </tr>
    <tr>
      <th>all_star</th>
      <td>0</td>
    </tr>
    <tr>
      <th>cpoy_rank</th>
      <td>340</td>
    </tr>
    <tr>
      <th>dpoy_rank</th>
      <td>337</td>
    </tr>
    <tr>
      <th>mip_rank</th>
      <td>349</td>
    </tr>
    <tr>
      <th>roy_rank</th>
      <td>345</td>
    </tr>
    <tr>
      <th>sixmoy_rank</th>
      <td>338</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>



Before we save the cleaned csv files. We check the data types to see if they are all properly laid out, which they are.


```python
df.dtypes #confirming data types
```




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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>player</th>
      <td>object</td>
    </tr>
    <tr>
      <th>age</th>
      <td>int64</td>
    </tr>
    <tr>
      <th>team</th>
      <td>object</td>
    </tr>
    <tr>
      <th>pos</th>
      <td>object</td>
    </tr>
    <tr>
      <th>g</th>
      <td>int64</td>
    </tr>
    <tr>
      <th>gs</th>
      <td>int64</td>
    </tr>
    <tr>
      <th>mp</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fga</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg_pct</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg3</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg3a</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg3_pct</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg2</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg2a</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fg2_pct</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>efg_pct</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>ft</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>fta</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>ft_pct</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>orb</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>drb</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>trb</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>ast</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>stl</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>blk</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>tov</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>pf</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>pts</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>player_additional</th>
      <td>object</td>
    </tr>
    <tr>
      <th>mvp_rank</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>all_nba</th>
      <td>bool</td>
    </tr>
    <tr>
      <th>all_star</th>
      <td>bool</td>
    </tr>
    <tr>
      <th>cpoy_rank</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>dpoy_rank</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>mip_rank</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>roy_rank</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>sixmoy_rank</th>
      <td>float64</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> object</label>



Now we will save the cleaned per_game dataset to a csv file


```python
df.to_csv("per_game_cleaned.csv", index = False)
```

We have succesfully cleaned the per_game dataset, now its time for the advanced.

We will first analyse `adv_df`



```python
adv_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 733 entries, 0 to 732
    Data columns (total 30 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   Rk                 733 non-null    int64  
     1   Player             733 non-null    object 
     2   Age                733 non-null    int64  
     3   Team               733 non-null    object 
     4   Pos                733 non-null    object 
     5   G                  733 non-null    int64  
     6   GS                 733 non-null    int64  
     7   MP                 733 non-null    int64  
     8   PER                733 non-null    float64
     9   TS%                730 non-null    float64
     10  3PAr               730 non-null    float64
     11  FTr                730 non-null    float64
     12  ORB%               733 non-null    float64
     13  DRB%               733 non-null    float64
     14  TRB%               733 non-null    float64
     15  AST%               733 non-null    float64
     16  STL%               733 non-null    float64
     17  BLK%               733 non-null    float64
     18  TOV%               731 non-null    float64
     19  USG%               733 non-null    float64
     20  OWS                733 non-null    float64
     21  DWS                733 non-null    float64
     22  WS                 733 non-null    float64
     23  WS/48              733 non-null    float64
     24  OBPM               733 non-null    float64
     25  DBPM               733 non-null    float64
     26  BPM                733 non-null    float64
     27  VORP               733 non-null    float64
     28  Awards             54 non-null     object 
     29  Player-additional  733 non-null    object 
    dtypes: float64(20), int64(5), object(5)
    memory usage: 171.9+ KB



```python
adv_df
```





  <div id="df-6a1fc685-f158-4194-91c6-8e3cd1325468" class="colab-df-container">
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
      <th>Rk</th>
      <th>Player</th>
      <th>Age</th>
      <th>Team</th>
      <th>Pos</th>
      <th>G</th>
      <th>GS</th>
      <th>MP</th>
      <th>PER</th>
      <th>TS%</th>
      <th>...</th>
      <th>OWS</th>
      <th>DWS</th>
      <th>WS</th>
      <th>WS/48</th>
      <th>OBPM</th>
      <th>DBPM</th>
      <th>BPM</th>
      <th>VORP</th>
      <th>Awards</th>
      <th>Player-additional</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Amen Thompson</td>
      <td>23</td>
      <td>HOU</td>
      <td>PG</td>
      <td>79</td>
      <td>79</td>
      <td>2953</td>
      <td>18.9</td>
      <td>0.594</td>
      <td>...</td>
      <td>6.5</td>
      <td>3.8</td>
      <td>10.3</td>
      <td>0.167</td>
      <td>1.6</td>
      <td>1.0</td>
      <td>2.6</td>
      <td>3.4</td>
      <td>DPOY-8</td>
      <td>thompam01</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Kevin Durant</td>
      <td>37</td>
      <td>HOU</td>
      <td>SF</td>
      <td>78</td>
      <td>78</td>
      <td>2840</td>
      <td>21.1</td>
      <td>0.641</td>
      <td>...</td>
      <td>7.5</td>
      <td>3.2</td>
      <td>10.7</td>
      <td>0.180</td>
      <td>4.4</td>
      <td>0.1</td>
      <td>4.5</td>
      <td>4.7</td>
      <td>CPOY-9ASNBA2</td>
      <td>duranke01</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Desmond Bane</td>
      <td>27</td>
      <td>ORL</td>
      <td>SG</td>
      <td>82</td>
      <td>82</td>
      <td>2756</td>
      <td>16.9</td>
      <td>0.607</td>
      <td>...</td>
      <td>4.8</td>
      <td>2.6</td>
      <td>7.4</td>
      <td>0.129</td>
      <td>1.6</td>
      <td>-0.2</td>
      <td>1.4</td>
      <td>2.3</td>
      <td>NaN</td>
      <td>banede01</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Toumani Camara</td>
      <td>25</td>
      <td>POR</td>
      <td>PF</td>
      <td>82</td>
      <td>82</td>
      <td>2731</td>
      <td>11.3</td>
      <td>0.578</td>
      <td>...</td>
      <td>2.1</td>
      <td>2.7</td>
      <td>4.8</td>
      <td>0.084</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>-1.0</td>
      <td>0.7</td>
      <td>NaN</td>
      <td>camarto01</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Jabari Smith Jr.</td>
      <td>22</td>
      <td>HOU</td>
      <td>PF</td>
      <td>77</td>
      <td>77</td>
      <td>2705</td>
      <td>13.5</td>
      <td>0.570</td>
      <td>...</td>
      <td>3.4</td>
      <td>3.3</td>
      <td>6.6</td>
      <td>0.118</td>
      <td>-0.5</td>
      <td>-0.2</td>
      <td>-0.7</td>
      <td>0.9</td>
      <td>NaN</td>
      <td>smithja05</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>728</th>
      <td>578</td>
      <td>Stanley Umude</td>
      <td>26</td>
      <td>SAS</td>
      <td>SG</td>
      <td>2</td>
      <td>0</td>
      <td>7</td>
      <td>11.6</td>
      <td>0.500</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.165</td>
      <td>-6.7</td>
      <td>7.1</td>
      <td>0.4</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>umudest01</td>
    </tr>
    <tr>
      <th>729</th>
      <td>579</td>
      <td>Colby Jones</td>
      <td>23</td>
      <td>DET</td>
      <td>SG</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>12.9</td>
      <td>0.333</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.124</td>
      <td>-3.2</td>
      <td>5.7</td>
      <td>2.4</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>jonesco02</td>
    </tr>
    <tr>
      <th>730</th>
      <td>580</td>
      <td>Noa Essengue</td>
      <td>19</td>
      <td>CHI</td>
      <td>PF</td>
      <td>2</td>
      <td>0</td>
      <td>6</td>
      <td>-11.9</td>
      <td>0.000</td>
      <td>...</td>
      <td>-0.1</td>
      <td>0.0</td>
      <td>-0.1</td>
      <td>-0.417</td>
      <td>-20.7</td>
      <td>-3.0</td>
      <td>-23.7</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>essenno01</td>
    </tr>
    <tr>
      <th>731</th>
      <td>581</td>
      <td>Trentyn Flowers</td>
      <td>20</td>
      <td>CHI</td>
      <td>SF</td>
      <td>2</td>
      <td>0</td>
      <td>6</td>
      <td>16.7</td>
      <td>0.667</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.023</td>
      <td>1.7</td>
      <td>-2.7</td>
      <td>-1.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>flowetr01</td>
    </tr>
    <tr>
      <th>732</th>
      <td>582</td>
      <td>Darius Brown II</td>
      <td>26</td>
      <td>CLE</td>
      <td>SG</td>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>-7.7</td>
      <td>0.000</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-0.269</td>
      <td>-16.3</td>
      <td>-2.4</td>
      <td>-18.7</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>brownda04</td>
    </tr>
  </tbody>
</table>
<p>733 rows × 30 columns</p>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-6a1fc685-f158-4194-91c6-8e3cd1325468')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-6a1fc685-f158-4194-91c6-8e3cd1325468 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-6a1fc685-f158-4194-91c6-8e3cd1325468');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>

    </div>
  </div>




Similar to the per game dataset, this dataset also contains the data of traded players, with combined rows being the first instance hence we drop the duplicates


```python
#Keep only the combines 2TM/3TM row for traded players, same as per_game
adv_df = adv_df.drop_duplicates(subset = "Player", keep = "first")
```

We would also drop `Rk` in this dataset


```python
#Rk is just a row number, so we drop it
adv_df = adv_df.drop(columns=["Rk"])
```

We would also format the columns to match python variable syntax and we would reset the indexes to account for dropped rows


```python
adv_df.columns = (adv_df.columns.str.lower().str.replace("%", "_pct").str.replace("/", "_per_").str.replace("-", "_"))
adv_df = adv_df.rename(columns={"3par": "fg3a_rate"})
adv_df = adv_df.reset_index(drop=True)
adv_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 582 entries, 0 to 581
    Data columns (total 29 columns):
     #   Column             Non-Null Count  Dtype  
    ---  ------             --------------  -----  
     0   player             582 non-null    object 
     1   age                582 non-null    int64  
     2   team               582 non-null    object 
     3   pos                582 non-null    object 
     4   g                  582 non-null    int64  
     5   gs                 582 non-null    int64  
     6   mp                 582 non-null    int64  
     7   per                582 non-null    float64
     8   ts_pct             582 non-null    float64
     9   fg3a_rate          582 non-null    float64
     10  ftr                582 non-null    float64
     11  orb_pct            582 non-null    float64
     12  drb_pct            582 non-null    float64
     13  trb_pct            582 non-null    float64
     14  ast_pct            582 non-null    float64
     15  stl_pct            582 non-null    float64
     16  blk_pct            582 non-null    float64
     17  tov_pct            582 non-null    float64
     18  usg_pct            582 non-null    float64
     19  ows                582 non-null    float64
     20  dws                582 non-null    float64
     21  ws                 582 non-null    float64
     22  ws_per_48          582 non-null    float64
     23  obpm               582 non-null    float64
     24  dbpm               582 non-null    float64
     25  bpm                582 non-null    float64
     26  vorp               582 non-null    float64
     27  awards             54 non-null     object 
     28  player_additional  582 non-null    object 
    dtypes: float64(20), int64(4), object(5)
    memory usage: 132.0+ KB


Advanced mp is total season minutes, not minutes per game, and age , team. pos, g, gs are already in the per-game data. We keep only the player ID plus the advanced metrics we need, so the merge doesn't create duplicate or confusing colums


```python
adv_keep = adv_df[["player_additional", "per", "ts_pct", "fg3a_rate", "usg_pct", "ws", "bpm"]]
adv_keep.isna().sum() #check missing valuesin what we kept
```




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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>player_additional</th>
      <td>0</td>
    </tr>
    <tr>
      <th>per</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ts_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg3a_rate</th>
      <td>0</td>
    </tr>
    <tr>
      <th>usg_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ws</th>
      <td>0</td>
    </tr>
    <tr>
      <th>bpm</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>



We merge onto the filtered per-game data using the unique player ID with a left join, so we keep all 350 filtered players and add their advanced stats.


```python
df = df.merge (adv_keep, on="player_additional", how="left")
print(df.shape)
df[["per", "ts_pct", "fg3a_rate", "usg_pct", "ws", "bpm"]].isna().sum() # except all 0
```

    (350, 44)





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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>per</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ts_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>fg3a_rate</th>
      <td>0</td>
    </tr>
    <tr>
      <th>usg_pct</th>
      <td>0</td>
    </tr>
    <tr>
      <th>ws</th>
      <td>0</td>
    </tr>
    <tr>
      <th>bpm</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div><br><label><b>dtype:</b> int64</label>




```python
df.isna().sum()
df.dtypes
df.to_csv("advanced_cleaned.csv", index=False)
```




```python

```


```python
from google.colab import drive
drive.mount('/content/drive')

!jupyter nbconvert --to markdown "/content/drive/MyDrive/Colab Notebooks/NBA-statistics-project.ipynb"
```


      File "/tmp/ipykernel_2580/2737331032.py", line 4
        jupyter nbconvert --to html /content/drive/MyDrive/Colab Notebooks/NBA-statistics-project.ipynb
                ^
    SyntaxError: invalid syntax


