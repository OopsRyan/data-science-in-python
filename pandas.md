## Pandas

### The Series Data Structure (one dimension)
	import pandas as pd
	animals = ['tiger', 'bear', 'moose']
	pd.Series(animals)

	##
	0    Tiger
	1     Bear
	2    Moose
	dtype: object

	numbers = [1, 2, None]
	pd.Series(numbers)

	##
	0    1.0
	1    2.0
	2    NaN
	dtype: float64

	dic = {'a':'1', 'b':'2', 'c':'3', 'd':'4'}
	s = pd.Series(dic)

	##
	a    1
	b    2
	c    3
	d    4
	dtype: object

	s.index
	## Index(['a', 'b', 'c', 'd'], dtype= 'object')

	s = pd.Series(['a', 'b', 'c', 'd'], index=['1', '2', '3', '4'])
	##
	1    a
	2    b
	3    c
	4    d
	dtype: object

	sports = {'Archery':'Bhutan', 'Golf':'Scotland', 'Sumo':'Japan', 'Taekwondo':'South Korea'}
	s = pd.Series(sports, index=['Golf', 'Sumo', 'Hockey'])
	##
	Golf      Scotland
	Sumo         Japan
	Hockey         NaN
	dtype: object

### Query a Series

	sports = {'Archery': 'Bhutan', 'Golf': 'Scotland', 'Sumo': 'Japan', 'Taekwondo': 'South Korea'}
	s = pd.Series(sports)

	#
	Archery           Bhutan
	Golf            Scotland
	Sumo               Japan
	Taekwondo    South Korea
	dtype: object

	s.iloc[3]
	# 
	'South Korea'

	s.loc['Golf']
	#
	'Scotland'

	s[3]
	#
	'South Korea'

	s['Golf']
	#
	'Scotland'

---
##### Efficiency Comparison between iteration and np.sum()
	s = pd.Series(np.random.randint(0,1000,10000))
	s.head() ## the top 5 lines
	len(s) ## 10000
	## iteration
	%% timeit -n 100
	summary = 0
	for item in s:
		summary += item
	
	# 100 loops, best of 3: 1.85 ms per loop

	%% timeit -n 100
	summary = np.sum(s)

	# 100 loops, best of 3: 104 µs per loop
---

`s += 2` adds two to each item in s using broadcasting

	s = pd.Series([1, 2, 3])
	s.loc['Animal'] = 'Bears'
	#
	0             1
	1             2
	2             3
	Animal    Bears
	dtype: object

Multiple same indexs must be defined in the way like *s2*, since the dictionary objects(*s1*) are set where indexs are unique.

	s1 = pd.Series({'a':'1', 'b':'2'}) 
	s2 = pd.Series(['3','4','5'], index=['c','c','c']) 
	s = s1.append(s2)
	s
	# 
	a    1
	b    2 
	c    3
	c    4
	c    5
	dtype: object

	s.loc['c']
	#
	c    3
	c    4
	c    5
	dtype: object

### The DataFrame

	purchase_1 = pd.Series({'Name':'Chris', 'Item Purchased':'Dog Food', 'Cost':22.50})
	purchase_2 = pd.Series({'Name':'Kevyn', 'Item Purchased':'Kitty Litter', 'Cost':2.50})
	purchase_1 = pd.Series({'Name':'Vinod', 'Item Purchased':'Bird Seed', 'Cost':5.00})
	df = pd.DataFrame([purchase_1, purchase_2, purchase_3], index = ['Store 1', 'Store 1', 'Store 2'])
	df.head()
	#
			Cost	Item Purchased	Name
	Store 1	22.5	Dog Food		Chris
	Store 1	2.5		Kitty Litter	Kevyn
	Store 2	5.0		Bird Seed		Vinod

	df.loc['Store2']
	#
	Cost                      5
	Item Purchased    Bird Seed
	Name                  Vinod
	Name: Store2, dtype: object

	type(df.loc['Store 2'])
	# pandas.core.series.Series

	df.loc['Store 1']
	#
			Cost	Item Purchased	Name
	Store 1	22.5	Dog Food		Chris
	Store 1	2.5		Kitty Litter	Kevyn

	df.loc['Store1', 'Cost']
	#
	Store 1    22.5
	Store 1     2.5
	Name: Cost, dtype: float64

	df.T
	#
					Store 1		Store 1			Store 2
	Cost			22.5		2.5				5
	Item Purchased	Dog Food	Kitty Litter	Bird Seed
	Name			Chris		Kevyn			Vinod

	df['Cost']
	#
	Store 1    22.5
	Store 1     2.5
	Store 2     5.0
	Name: Cost, dtype: float64

	df.loc['Store 1']['Cost']
	#
	Store 1    22.5
	Store 1     2.5
	Name: Cost, dtype: float64

	df.loc[:, ['Name','Cost']]
	#
			Name	Cost
	Store 1	Chris	22.5
	Store 1	Kevyn	2.5
	Store 2	Vinod	5.0

	df.drop('Store 1')
	#
			Cost	Item Purchased	Name
	Store 2	5.0		Bird Seed		Vinod

	copy_df = df.copy()
	copy_df = copy_df.drop('Store 1')
	copy_df
	#
			Cost	Item Purchased	Name
	Store 2	5.0		Bird Seed		Vinod

	del copy_df['Name']
	copy_df
	#
			Cost	Item Purchased	Name
	Store 2	5.0		Bird Seed		Vinod

	df['Location'] = None
	#
			Cost	Item Purchased	Name   Location
	Store 1	22.5	Dog Food		Chris  None
	Store 1	2.5		Kitty Litter	Kevyn  None
	Store 2	5.0		Bird Seed		Vinod  None


### DataFrame indexing and loading
	costs = df['Cost']
	#
	Store 1    22.5
	Store 1     2.5
	Store 2     5.0
	Name: Cost, dtype: float64

	costs += 2
	#
	Store 1    24.5
	Store 1     4.5
	Store 2     7.0
	Name: Cost, dtype: float64

	df
	#
			Cost	Item Purchased	Name   Location
	Store 1	24.5	Dog Food		Chris  None
	Store 1	4.5		Kitty Litter	Kevyn  None
	Store 2	7.0		Bird Seed		Vinod  None


	df = pd.read_csv('aa.csv', index_col=0, skiprow=1)
	df.head()

	df.columns

	# modify column names
	for col in df.columns:
		if col[:2] == '01':
			df.rename(columns={col:'Gold'}, inplace=True) 

### Querying a DataFrame
	df['Gold'] > 0
	#
	Afghanistan (AFG)                               False
	Algeria (ALG)                                    True
	Argentina (ARG)                                  True
	Name: Gold, dtype: bool

	only_gold = df.where(df['Gold'] > 0)
	# DataFrame

	only_gold['Gold'].count()
	# 100

	df['Gold'].count()
	147

	# drop those rows with NaN data
	only_gold = only_gold.dropna()

	only_gold = df[df['Gold'] > 0]
	only_gold.head()
	# DataFrame

	len( df[ (df['Gold'] > 0) | (df['Gold.1'] > 0)])
	# 101

	df[ (df['Gold.1'] >0) | (df['Gold'] == 0)]
	# DataFrame


### Indexing DataFrames
	df.head()
	#
		q		w		e
	0   ads  	 		
	1	zxc
	2	asd

	# change the index to the dimension q, create a new column using the index a, or the data of a will miss
	df['a'] = df.index
	df = df.set_index('q')
	df.head()
	#
		w		e 		a
	q      	 		
	ads 				0
	zxc 				1
	asd 				2

	# reset the index
	df = df.reset_index()
	#
		q		w		e 		a
	0   ads 					0  	 		
	1	zxc						1
	2	asd 					2

	df = pd.read_csv('bb.csv')
	#
		sum 	iii
	0 	40		s
	1	50		s
	2	50		x

	df['sum'].unique()
	# array([40, 50])

	df = df[df['sum'] == 50]
	#
		sum 	iii
	1	50		s
	2	50		s

	columns_to_keep = ['sum']
	df = df[columns_to_keep]
	#
		sum
	1	50
	2	50

### Missing Values
	df = pd.read_csv('log.csv')
	#
		a 		b 		c
	0	False 	10.0	10
	1	NaN		NaN		13
	2	NaN		NaN		7
	3	NaN		NaN		4

	df = df.set_index('c')
	df = df.sort_index()
	#
		a 		b
	c
	4   NaN		NaN
	7	NaN		NaN
	10	False	10.0
	13	NaN		NaN

	df = df.fillna(method='ffill')
	#
		a 		b
	c
	4   False	10.0
	7	False	10.0
	10	False	10.0
	13	False	10.0

---






