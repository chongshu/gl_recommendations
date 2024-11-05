# Dataset for Glass Lewis Recommendations

### Suggested citation: 


Shu, Chong. "The Proxy Advisory Industry: Influencing and Being Influenced." <i>Journal of Financial Economics</i> 154, (2024): 103810. <a>https://www.sciencedirect.com/science/article/pii/S0304405X24000333 </a>
 
 



### 1. Download dataset

&nbsp;&nbsp;&nbsp;&nbsp; <b><a href="./Public Record Request/processed_GL_recommendations.csv">data.csv</a></b>


### 2. Variable definitions

<div align="center">

| Variable        | Label                                                  |
|-----------------|--------------------------------------------------------|
| `itemonagendaid`| Unique election item identifier (ISS Voting Analytics) |
| `GL_rec`        | Glass Lewis Recommendations                            |

</div>

### 3. Raw data source

Glass Lewis’s recommendations from 2008 to 2021 were obtained through a North Carolina Public Records Law request to the state's Department of State Treasurer. The request included the following information:

1. The name of the public pension fund’s proxy advisor.
2. The recommendations provided by this advisor (both generic and customized).
3. The votes cast by the fund.

The Department of State Treasurer responded with the following files: 

&nbsp;&nbsp;&nbsp;&nbsp; <b><a href="./Public Record Request">Public Record Request (2008 - 2021)</a></b>


#### Example of response

<img src="./figures/example.png" style="border: 2px solid black;" />
 
### 4. Match with ISS voting analytics dataset

#### Step one: matching companies

The Glass Lewis recommendations obtained through the Public Records Law request lack company identifiers such as tickers or CUSIPs, containing only company names. ISS and Glass Lewis occasionally use different names for the same company.


- For example, in ISS Voting Analytics, "Apple Inc." is used consistently throughout the sample years, while Glass Lewis referred to the company as "Apple Computer Inc." in earlier years.

To match companies between Glass Lewis recommendations and ISS Voting Analytics, the initial step involves precisely matching each company name in Glass Lewis’s recommendations to those appearing in Glass Lewis customers' N-PX forms.

- This allows for identifying the company’s ticker. An underlying assumption here is that company names in the Glass Lewis voting system remain consistent across different customers.

These tickers are then used to match companies between the Glass Lewis recommendation dataset and ISS Voting Analytics.


#### Step two: matching proposals

Matching annual meetings between Glass Lewis’s recommendations and ISS Voting Analytics requires a one-to-one correspondence between the ticker and meeting date across both datasets. 

A challenge arises in matching election items, as ISS and Glass Lewis use different styles for subitem numbers in director elections.

- For instance, at the 2013 Starbucks annual meeting, Glass Lewis assigns Howard Schultz’s election the item number “1,” while ISS labels the same election as “1a.” This discrepancy can result in numerous mismatches if matching relies solely on item numbers.

To improve matching accuracy, I implement the following steps:
 
1. When both ISS and Glass Lewis use the same style (either numeric or alphanumeric) for annual meetings, I match items based on the item number.

2. If ISS and Glass Lewis use different styles, I match proposals by sequence number, provided the total number of proposals for the annual meeting is the same in both datasets.

3. In cases where ISS and Glass Lewis use different styles and the total number of proposals for an annual meeting are different between datasets, I classify these as errors and exclude them. Of the 18,156 annual meetings in the sample, 179 instances occur where the total number of proposals differs between the two datasets.
 