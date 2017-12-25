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

###
