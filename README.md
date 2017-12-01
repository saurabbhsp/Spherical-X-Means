# Spherical X-Means

This is a POC of a Spherical version of XMeans outlined in ["Extending K-means with Effecient Estimation of Number of Clusters"](https://www.cs.cmu.edu/~dpelleg/download/xmeans.pdf), Pelleg et al., CMU.

Distributed representations of documents tend to cluster similar texts along certain directions. Spherical X-Means can hence be used to cluster and identify topics/themes in document vector space. This algorithm is validated on a sample of normalised document vectors generated by training [Gensim Doc2Vec](https://radimrehurek.com/gensim/models/doc2vec.html) on a larger corpus of news articles.

## Algorithmic Updates

1. The algorithm makes use of [Spherical K-means](https://github.com/clara-labs/spherecluster) instead of vanilla K-Means to cluster data on unit hypersphere.

2. At each level, the algorithm spawns "maxClusters" number of Spherical K-means and selects a model based on their BIC scores.

3. Arc length is the measure of distance between two points on a sphere. On a unit sphere, arc length between two points is equal to the angle between them. Hence, the algorithm uses Sum of Squared Angles (in Degrees) to compute BIC instead of Sum of Squared Errors.

## Usage

```
# Find appropriate number of clusters in data array X (n_examples x n_features)
from XMeans import XMeansTraining

Centers,Labels = XMeansTraining(X,maxBranching = 5,norm = True) # set norm = False if X is not normalized
```	

## Example

"sampleData.json" provides 150 dimensional normalized document vectors of 11957 news articles along with their titles. Refer "Example.ipynb" for implementation details. Spherical X-Means produces 26 clusters on this data. "clusterID.txt"s at "./Clusters" contain titles of articles clustered together. 

A 3D tSNE visualization of clustered document vectors:

![](https://github.com/sagarkurandwad/Spherical-X-Means/raw/master/GIF/3D_visualization.gif)


### Hits

Some of the clusters whose themes could be identified clearly were:


| ClusterID |                                                                                                                                                                                                                                               Titles                                                                                                                                                                                                                                               |        Themes        |
|:---------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:--------------------:|
|     0     |                                                                                                                    India's annual mean temperature rises by 1.2 degree since 1901<br/>Menopause is not a pause in your life<br/>New disposable patch can help detect sleep apnea<br/>How You Can Save More Lives By Eating Beef Instead of Chicken<br/>Cycle your way to a healthier you                                                                                                                   |        Health        |
|     3     |Countdown begins for ISRO's GSLV-Mark III rocket launch - ANI News<br/>ISRO launches India's heaviest rocket GSLV-Mk III from Sriharikota | Oneindia News<br/>ISRO ready to power the Monster Rocket<br/>NASA to launch new manned Mars rovers in 2020<br/>India's Heaviest Rocket GSLV-MkIII D1 & Heaviest Satellite GSAT-19 Successfully Launched; 4 Reasons It's A Big Deal                                                         |         Space        |
|     7     |                                                     Apple's iOS 11 Files app pops up on App Store ahead of WWDC<br/>New iOS, Siri Speakers & More Expected at Apple's WWDC Tonight<br/>Samsung Galaxy S8 'Pirates of the Caribbean' edition goes on sale; priced around Rs 56,700<br/>Moto X Play gets Android 7.1.1 Nougat in India; is it soak test or public roll-out?<br/>Sony Xperia XA1 Ultra listed on the Sony India website; might launch soon                                                    |     Smart Phones     |
|     9     | England vs New Zealand, ICC Champions Trophy 2017: Eoin Morgan says England 'completely different' since 2015 World Cup<br/>ICC Champions Trophy: Bangladesh Team heckled at Iftar Party in England | Oneindia News<br/>India vs Pakistan: Vijay Goel congratulates Indian cricket team<br/>Sensors Are Being Used In Bats In The Champions Trophy. Here's Everything You Need To Know About Them<br/>ICC Champion trophy: Vijay Mallya spotted with Sunil Gavaskar during Ind VS Pak match |Oneindia News | ICC Champions Trophy |
|     12    |                                                                                              Rohr, Eagles skipper meet NFF bosses over cash<br/>Super Eagles Tormentor Rantie, Six Other Bafana Stars Doubtful For Uyo Clash<br/>Real Madrid: 'Insatiable' Real hailed masters of Europe<br/>La Liga: Girona promoted to Spanish top flight for first time<br/>Bhaichung backs franchise fee waiver for EB, MB                                                                                             |       Football       |
|     14    |                       After Padmavati, SLB and Salman will collaborate for a film<br/>JUST HOT! Prabhas' New Look Goes Viral; Anushka Shetty & Pooja Hegde's FIGHT For Saaho Continues!<br/>Munna Michael trailer: 5 moments that prove Tiger Shroff-Nawazuddin Siddiqui have arrived with something exciting<br/>Vidya Balan and her father to join hands with Farhan Akhtar<br/>MOM APPROVED! Sara Ali Khan On A Dinner Date With Sushant Singh Rajput; Amrita Singh Joins Them Too                      |       Bollywood      |
|     24    |                            Saudi shuts Al-Jazeera office in Qatar row: Ministry<br/> Saudi Arabia, Bahrain, Egypt, UAE cut ties with Qatar for supporting terror groups<br/>Lai Mohammed: 'Those behind fake news will be prosecuted', Minister says<br/>Emirates' flights connecting Doha suspended after Saudi-Qatar row<br/>Qatar denounces 'unjustified' cut of Gulf tiesQatar slams Saudi Arabia, UAE, Egypt's decision to cut diplomatic ties, calls it 'campaign of lies'                           |     Qatar Crisis     |

### Misses

Following were some clusters whose themes were difficult to name:

| ClusterID |                                                                                                                                                                                                   Titles                                                                                                                                                                                                   | Probable Themes |
|:---------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:---------------:|
|     2     | Bangalore's free Wi-Fi services to entertain bus traveler during their journey<br/>Trump Effect: IT Employees express interest in Agriculture<br/>Prime Minister Narendra Modi's Tweets On World Environmental Day<br/>Will it be the end of Bengaluru's iconic cinema house, Fame Shankarnag?<br/>Railways to reduce dependence on diesel<br/>Joseph Muscat: Labour party leader sworn in as Malta's prime minister | Current Affairs |
|     21    |                                                                  Hyderabad: After Sonu Nigam, city complaints of Noise pollution<br/>Shiv Sena slams Maharashtra govt over farmers' stir<br/>Kerala plants one crore saplings on environment day<br/>MCC observes World Environment Day<br/>State moots to make Aadhar mandatory for organ donors?                                                                 |   Environment   |






