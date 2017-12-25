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































