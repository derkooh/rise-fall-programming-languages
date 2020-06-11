
## 1. Data on tags over time
<p>How can we tell what programming languages and technologies are used by the most people? How about what languages are growing and which are shrinking, so that we can tell which are most worth investing time in?</p>
<p>One excellent source of data is <a href="https://stackoverflow.com/">Stack Overflow</a>, a programming question and answer site with more than 16 million questions on programming topics. By measuring the number of questions about each technology, we can get an approximate sense of how many people are using it. We're going to use open data from the <a href="https://data.stackexchange.com/">Stack Exchange Data Explorer</a> to examine the relative popularity of languages like R, Python, Java and Javascript have changed over time.</p>
<p>Each Stack Overflow question has a <strong>tag</strong>, which marks a question to describe its topic or technology. For instance, there's a tag for languages like <a href="https://stackoverflow.com/tags/r">R</a> or <a href="https://stackoverflow.com/tags/python">Python</a>, and for packages like <a href="https://stackoverflow.com/questions/tagged/ggplot2">ggplot2</a> or <a href="https://stackoverflow.com/questions/tagged/pandas">pandas</a>.</p>
<p><img src="https://assets.datacamp.com/production/project_435/img/tags.png" alt="Stack Overflow tags"></p>
<p>We'll be working with a dataset with one observation for each tag in each year. The dataset includes both the number of questions asked in that tag in that year, and the total number of questions asked in that year.</p>


```R
# Load libraries
library(readr)
library(dplyr)

# Load dataset
by_tag_year <- read_csv('datasets/by_tag_year.csv')

# Inspect the dataset
by_tag_year
```
<table>
<caption>A spec_tbl_df: 40518 x 4</caption>
<thead>
	<tr><th scope=col>year</th><th scope=col>tag</th><th scope=col>number</th><th scope=col>year_total</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2008</td><td>.htaccess           </td><td>  54</td><td>58390</td></tr>
	<tr><td>2008</td><td>.net                </td><td>5910</td><td>58390</td></tr>
	<tr><td>2008</td><td>.net-2.0            </td><td> 289</td><td>58390</td></tr>
	<tr><td>2008</td><td>.net-3.5            </td><td> 319</td><td>58390</td></tr>
	<tr><td>2008</td><td>.net-4.0            </td><td>   6</td><td>58390</td></tr>
	<tr><td>2008</td><td>.net-assembly       </td><td>   3</td><td>58390</td></tr>
	<tr><td>2008</td><td>.net-core           </td><td>   1</td><td>58390</td></tr>
	<tr><td>2008</td><td>2d                  </td><td>  42</td><td>58390</td></tr>
	<tr><td>2008</td><td>32-bit              </td><td>  19</td><td>58390</td></tr>
	<tr><td>2008</td><td>32bit-64bit         </td><td>   4</td><td>58390</td></tr>

	<tr><td>...</td><td>...</td><td>...</td><td>...</td></tr>
r>
	<tr><td>2018</td><td>z-index          </td><td> 107</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zip              </td><td> 410</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zipfile          </td><td> 115</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zk               </td><td>  35</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zlib             </td><td>  89</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zoom             </td><td> 196</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zsh              </td><td> 175</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zurb-foundation  </td><td> 182</td><td>1085170</td></tr>
	<tr><td>2018</td><td>zxing            </td><td>  95</td><td>1085170</td></tr>
</tbody>
</table>


## 2. Now in fraction format
<p>This data has one observation for each pair of a tag and a year, showing the number of questions asked in that tag in that year and the total number of questions asked in that year. For instance, there were 54 questions asked about the <code>.htaccess</code> tag in 2008, out of a total of 58390 questions in that year.</p>
<p>Rather than just the counts, we're probably interested in a percentage: the fraction of questions that year that have that tag. So let's add that to the table.</p>


```R
# Add fraction column
by_tag_year_fraction <- mutate(by_tag_year, fraction=number / year_total)

# Print the new table
by_tag_year_fraction
```


<table>
<caption>A spec_tbl_df: 40518 x 5</caption>
<thead>
	<tr><th scope=col>year</th><th scope=col>tag</th><th scope=col>number</th><th scope=col>year_total</th><th scope=col>fraction</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2008</td><td>.htaccess           </td><td>  54</td><td>58390</td><td>9.248159e-04</td></tr>
	<tr><td>2008</td><td>.net                </td><td>5910</td><td>58390</td><td>1.012160e-01</td></tr>
	<tr><td>2008</td><td>.net-2.0            </td><td> 289</td><td>58390</td><td>4.949478e-03</td></tr>
	<tr><td>2008</td><td>.net-3.5            </td><td> 319</td><td>58390</td><td>5.463264e-03</td></tr>
	<tr><td>2008</td><td>.net-4.0            </td><td>   6</td><td>58390</td><td>1.027573e-04</td></tr>
	<tr><td>2008</td><td>.net-assembly       </td><td>   3</td><td>58390</td><td>5.137866e-05</td></tr>
	<tr><td>2008</td><td>.net-core           </td><td>   1</td><td>58390</td><td>1.712622e-05</td></tr>
	<tr><td>2008</td><td>2d                  </td><td>  42</td><td>58390</td><td>7.193013e-04</td></tr>
	<tr><td>2008</td><td>32-bit              </td><td>  19</td><td>58390</td><td>3.253982e-04</td></tr>
	<tr><td>2008</td><td>32bit-64bit         </td><td>   4</td><td>58390</td><td>6.850488e-05</td></tr>
	<tr><td>...</td><td>...</td><td>...</td><td>...</td><td>...</td></tr>
	<tr><td>2018</td><td>zip              </td><td> 410</td><td>1085170</td><td>3.778210e-04</td></tr>
	<tr><td>2018</td><td>zipfile          </td><td> 115</td><td>1085170</td><td>1.059742e-04</td></tr>
	<tr><td>2018</td><td>zk               </td><td>  35</td><td>1085170</td><td>3.225301e-05</td></tr>
	<tr><td>2018</td><td>zlib             </td><td>  89</td><td>1085170</td><td>8.201480e-05</td></tr>
	<tr><td>2018</td><td>zoom             </td><td> 196</td><td>1085170</td><td>1.806169e-04</td></tr>
	<tr><td>2018</td><td>zsh              </td><td> 175</td><td>1085170</td><td>1.612651e-04</td></tr>
	<tr><td>2018</td><td>zurb-foundation  </td><td> 182</td><td>1085170</td><td>1.677157e-04</td></tr>
	<tr><td>2018</td><td>zxing            </td><td>  95</td><td>1085170</td><td>8.754389e-05</td></tr>
</tbody>
</table>


## 3. Has R been growing or shrinking?
<p>So far we've been learning and using the R programming language. Wouldn't we like to be sure it's a good investment for the future? Has it been keeping pace with other languages, or have people been switching out of it?</p>
<p>Let's look at whether the fraction of Stack Overflow questions that are about R has been increasing or decreasing over time.</p>


```R
# Filter for R tags
r_over_time <- filter(by_tag_year_fraction,tag == 'r')

# Print the new table
r_over_time
```


<table>
<caption>A spec_tbl_df: 11 x 5</caption>
<thead>
	<tr><th scope=col>year</th><th scope=col>tag</th><th scope=col>number</th><th scope=col>year_total</th><th scope=col>fraction</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>2008</td><td>r</td><td>    8</td><td>  58390</td><td>0.0001370098</td></tr>
	<tr><td>2009</td><td>r</td><td>  524</td><td> 343868</td><td>0.0015238405</td></tr>
	<tr><td>2010</td><td>r</td><td> 2270</td><td> 694391</td><td>0.0032690516</td></tr>
	<tr><td>2011</td><td>r</td><td> 5845</td><td>1200551</td><td>0.0048685978</td></tr>
	<tr><td>2012</td><td>r</td><td>12221</td><td>1645404</td><td>0.0074273552</td></tr>
	<tr><td>2013</td><td>r</td><td>22329</td><td>2060473</td><td>0.0108368321</td></tr>
	<tr><td>2014</td><td>r</td><td>31011</td><td>2164701</td><td>0.0143257660</td></tr>
	<tr><td>2015</td><td>r</td><td>40844</td><td>2219527</td><td>0.0184021190</td></tr>
	<tr><td>2016</td><td>r</td><td>44611</td><td>2226072</td><td>0.0200402323</td></tr>
	<tr><td>2017</td><td>r</td><td>54415</td><td>2305207</td><td>0.0236052554</td></tr>
	<tr><td>2018</td><td>r</td><td>28938</td><td>1085170</td><td>0.0266667895</td></tr>
</tbody>
</table>


## 4. Visualizing change over time
<p>Rather than looking at the results in a table, we often want to create a visualization. Change over time is usually visualized with a line plot.</p>


```R
# Load ggplot2
library(ggplot2)

# Create a line plot of fraction over time
ggplot(r_over_time, aes(x = year, y = fraction)) +
  geom_line()
```


![png](output_10_0.png)




## 5. How about dplyr and ggplot2?
<p>Based on that graph, it looks like R has been growing pretty fast in the last decade. Good thing we're practicing it now!</p>
<p>Besides R, two other interesting tags are dplyr and ggplot2, which we've already used in this analysis. They both also have Stack Overflow tags!</p>
<p>Instead of just looking at R, let's look at all three tags and their change over time. Are each of those tags increasing as a fraction of overall questions? Are any of them decreasing?</p>


```R
# A vector of selected tags
selected_tags <- c("r","ggplot2","dplyr")

# Filter for those tags
selected_tags_over_time <- by_tag_year_fraction %>% filter(tag %in% selected_tags)

# Plot tags over time on a line plot using color to represent tag
ggplot(selected_tags_over_time, aes(x = year, y = fraction, colour = tag)) +
  geom_line()
```


![png](output_13_0.png)





## 6. What are the most asked-about tags?
<p>It's sure been fun to visualize and compare tags over time. The dplyr and ggplot2 tags may not have as many questions as R, but we can tell they're both growing quickly as well.</p>
<p>We might like to know which tags have the most questions <em>overall</em>, not just within a particular year. Right now, we have several rows for every tag, but we'll be combining them into one. That means we want <code>group_by()</code> and <code>summarize()</code>.</p>
<p>Let's look at tags that have the most questions in history.</p>


```R
# Find total number of questions for each tag
sorted_tags <- by_tag_year %>%
  group_by(tag) %>%
  summarize(tag_total=sum(number)) %>%
  arrange(desc(tag_total))

# Print the new table
sorted_tags
```


<table>
<caption>A tibble: 4080 x 2</caption>
<thead>
	<tr><th scope=col>tag</th><th scope=col>tag_total</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>javascript   </td><td>1632049</td></tr>
	<tr><td>java         </td><td>1425961</td></tr>
	<tr><td>c#           </td><td>1217450</td></tr>
	<tr><td>php          </td><td>1204291</td></tr>
	<tr><td>android      </td><td>1110261</td></tr>
	<tr><td>python       </td><td> 970768</td></tr>
	<tr><td>...</td><td>...</td></tr>
	<tr><td>lattice            </td><td>1003</td></tr>
	<tr><td>directory-structure</td><td>1002</td></tr>
	<tr><td>relation           </td><td>1002</td></tr>
	<tr><td>doctype            </td><td>1001</td></tr>
	<tr><td>rvest              </td><td>1001</td></tr>
	<tr><td>tableviewcell      </td><td>1000</td></tr>
	<tr><td>yahoo              </td><td>1000</td></tr>
</tbody>
</table>




## 7. How have large programming languages changed over time?
<p>We've looked at selected tags like R, ggplot2, and dplyr, and seen that they're each growing. What tags might be <em>shrinking</em>? A good place to start is to plot the tags that we just saw that were the most-asked about of all time, including JavaScript, Java and C#.</p>


```R
by_tag_subset
```



```R
# Get the six largest tags
highest_tags <- head(sorted_tags$tag,6)

# Filter for the six largest tags
by_tag_subset <- by_tag_year_fraction %>%
  filter(tag %in% highest_tags)

#by_tag_subset
# Plot tags over time on a line plot using color to represent tag
ggplot(by_tag_subset, aes(x = year, y = fraction, colour = tag)) +
  geom_line()
```


![png](output_20_0.png)




## 8. Some more tags!
<p>Wow, based on that graph we've seen a lot of changes in what programming languages are most asked about. C# gets fewer questions than it used to, and Python has grown quite impressively.</p>
<p>This Stack Overflow data is incredibly versatile. We can analyze <em>any</em> programming language, web framework, or tool where we'd like to see their change over time. Combined with the reproducibility of R and its libraries, we have ourselves a powerful method of uncovering insights about technology.</p>
<p>To demonstrate its versatility, let's check out how three big mobile operating systems (Android, iOS, and Windows Phone) have compared in popularity over time. But remember: this code can be modified simply by changing the tag names!</p>


```R
# A vector of selected tags
my_tags <- c("android","ios","windows-phone")

# Filter for those tags
by_tag_subset <- by_tag_year_fraction %>%
                    filter(tag %in% my_tags)

# Plot tags over time on a line plot using color to represent tag
ggplot(by_tag_subset, aes(x = year, y = fraction, colour = tag)) +
  geom_line()
```


![png](output_23_0.png)



