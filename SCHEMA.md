# References
[http://redis.io/commands](http://redis.io/commands)

[http://redis.io/topics/pipelining](http://redis.io/topics/pipelining)

[http://redis.io/topics/transactions](http://redis.io/topics/transactions)

[http://redis.io/topics/persistence](http://redis.io/topics/persistence)

[http://redis.io/topics/replication](http://redis.io/topics/replication)

[http://redis.io/topics/benchmarks](http://redis.io/topics/benchmarks)

[http://lethain.com/notes-on-redis-memory-usage](http://lethain.com/notes-on-redis-memory-usage)

# Redis Config

## To ENABLE Append Only File (AOF) run the following command in the Redis installation folder:
	$ redis-cli config set appendonly yes

## To DISABLE Append Only File (AOF) run the following command in the Redis installation folder:
	$ redis-cli config set save ""


# Redis Hello World

### app.js
	var redis = require('redis');
	var client = redis.createClient();
	client.on('error', function(err){
		console.log('Something went wrong ', err)
	});
	client.set('my test key', 'my test value', redis.print);
	client.get('my test key', function(error, result) {
		if (error) throw error;
		console.log('GET result ->', result)
	});

	$ node app.js
	$ redis-cli get 'my test key'

---

# HASHES aka. HSET
	HSET, HMSET, HSETNX, HGET, HMGET, HDEL, HEXISTS, HLEN, HKEYS, HVALS, HGETALL
# INDEXES aka. SET
	SADD, SMEMBERS, SREM, SMOVE, SPOP, SRANDMEMBER, SCARD, SORT, SDIFF, SDIFFSTORE, SINTER, SINTERSTORE, SUNION, SUNIONSTORE, SISMEMBER
# INDEXES - SORTED aka. ZSET
	ZADD, ZRANGE, ZREM, ZSCORE, ZRANGEBYSCORE, ZREMRANGEBYRANK, ZREMRANGEBYSCORE, ZRANK, ZCARD, ZCOUNT, ZINCRBY, ZINTERSTORE, ZUNIONSTORE, ZREVRANGE, ZREVRANGEBYSCORE, ZREVRANK

# GEO
A geographic coordinate point, meta-data, and sequence relationships.

## Hash
	MULTI
	HSET	geo:<id>	created		<value>	//timestamp
	HSET	geo:<id>	modified	<value>	//timestamp
	HSET	geo:<id>	latdeg		<value>
	HSET	geo:<id>	latdir		<value>
	HSET	geo:<id>	latrad		<value>
	HSET	geo:<id>	londeg		<value>
	HSET	geo:<id>	londir		<value>
	HSET	geo:<id>	lonrad		<value>
	HSET	geo:<id>	prev		<value> //list
	HSET	geo:<id>	next		<value> //list
	EXEC

## Indexes
	SADD	geos	<gid>

## Indexes - Sorted
	ZADD	geos:bycreated		[HGET gid:<id> created]		<gid>
	ZADD	geos:bymodified		[HGET gid:<id> modified]	<gid>

## Workers
	IF NIL == ZRANK geos:bycreated [gid in SMEMBERS geos]
		ZADD geos:bycreated [HGET gid:<id> created]	<gid>

	IF NIL == ZRANK geos:bymodified [gid in SMEMBERS geos]
		ZADD geos:bymodified [HGET gid:<id> modified]	<gid>
	-- Need to think about race conditions (WATCH?)


# EVENT
A temporal occurrence between two parties.

## Hash
	MULTI
	HSET	event:<id>	created		<value>	//timestamp
	HSET	event:<id>	modified	<value>	//timestamp
	HSET	event:<id>	type		<value> //const
	HSET	event:<id>	date		<value> //timestamp
	HSET	event:<id>	a_party		<value> //list
	HSET	event:<id>	b_party		<value> //list
	EXEC

## Indexes
	SADD	event	<eid>

## Indexes - Sorted
	ZADD	event:bycreated		[HGET eid:<id> created]		<eid>
	ZADD	event:bymodified	[HGET eid:<id> modified]	<eid>
	ZADD	event:bytype		[HGET eid:<id> modified]	<eid>
	

# PARCEL
A geographic area represented by perimeter geographic point coordinates.

## Hash
	MULTI
	HSET	parcel:<id>	created		<value>	//timestamp
	HSET	parcel:<id>	modified	<value>	//timestamp
	HSET	parcel:<id>	origin		<value>
	HSET	parcel:<id>	circmt		<value>
	HSET	parcel:<id>	areasqmt	<value>
	HSET	parcel:<id>	map			<value>
	HSET	parcel:<id>	lot			<value> //map+lot
	HSET	parcel:<id>	sub			<value> //map+lot+sub
	HSET	parcel:<id>	points		<value> //list
	HSET	parcel:<id>	events		<value> //list
	EXEC

## Indexes
	SADD	parcels	<pid>

## Indexes - Sorted
	ZADD	parcels:bycreated		[HGET pid:<id> created]		<pid>
	ZADD	parcels:bymodified		[HGET pid:<id> modified]	<pid>
	ZADD	parcels:bymap			[HGET pid:<id> map]			<pid>
	ZADD	parcels:bymaplot		[HGET pid:<id> maplot]		<pid>
	ZADD	parcels:bymapsublot		[HGET pid:<id> mapsublot]	<pid>
	
	
# STRUCTURE
A structural area represented by perimeter geographic point coordinates.

## Hash
	MULTI
	HSET	struct:<id>	created		<value>	//timestamp
	HSET	struct:<id>	modified	<value>	//timestamp
	HSET	struct:<id>	label		<value>
	HSET	struct:<id>	circmt		<value>
	HSET	struct:<id>	areasqmt	<value>
	HSET	struct:<id>	points		<value> //list
	HSET	struct:<id>	events		<value> //list
	EXEC

## Indexes
	SADD	structs	<sid>

## Indexes - Sorted
	ZADD	struct:bycreated		[HGET sid:<id> created]		<sid>
	ZADD	struct:bymodified		[HGET sid:<id> modified]	<sid>


# DEED
An instance of a recorded deed.

## Hash
	MULTI
	HSET	deed:<id>	created		<value>	//timestamp
	HSET	deed:<id>	modified	<value>	//timestamp
	HSET	deed:<id>	date		<value> //timestamp
	HSET	deed:<id>	type		<value> //const
	HSET	deed:<id>	county		<value>
	HSET	deed:<id>	book		<value> //county+book
	HSET	deed:<id>	page		<value> //county+book+page
	HSET	deed:<id>	img			<value> 
	HSET	deed:<id>	events		<value> //list
	HSET	deed:<id>	parcels		<value> //list
	EXEC

## Indexes
	SADD	deeds	<sid>

## Indexes - Sorted
	ZADD	deed:bycreated		[HGET sid:<id> created]		<sid>
	ZADD	deed:bymodified		[HGET sid:<id> modified]	<sid>
	ZADD	deed:bydate			[HGET sid:<id> date]		<sid>
	ZADD	deed:bytype			[HGET sid:<id> type]		<sid>
	ZADD	deed:bycounty		[HGET sid:<id> county]		<sid>
	ZADD	deed:bybook			[HGET sid:<id> book]		<sid>
	ZADD	deed:bypage			[HGET sid:<id> page]		<sid>


# PERSON
An instance of a person in history, their meta-data and related events.

## Hash
	MULTI
	HSET	person:<id>	created		<value>	//timestamp
	HSET	person:<id>	modified	<value>	//timestamp
	HSET	person:<id>	f_name   	<value> 
	HSET	person:<id>	m_name   	<value> 
	HSET	person:<id>	l_name		<value>
	HSET	person:<id>	gender		<value> //const
	HSET	person:<id>	birth		<value> //list
	HSET	person:<id>	death		<value> //list 
	HSET	person:<id>	marriage	<value> //list
	HSET	person:<id>	children	<value> //list
	HSET	person:<id>	estate		<value> //list
	EXEC

## Indexes
	SADD	persons	<pid>

## Indexes - Sorted
	ZADD	person:bycreated		[HGET pid:<id> created]		<pid>
	ZADD	person:bymodified		[HGET pid:<id> modified]	<pid>
	ZADD	person:bylname			[HGET pid:<id> l_name]		<pid>
	ZADD	person:bygender			[HGET pid:<id> gender]		<pid>


# TIMELINE
An chronological sequence of events.

## Hash
	MULTI
	HSET	timeline:<id>	created		<value>	//timestamp
	HSET	timeline:<id>	modified	<value>	//timestamp
	HSET	timeline:<id>	type   		<value> //const
	HSET	timeline:<id>	events		<value> //list
	EXEC

## Indexes
	SADD	timeline	<tid>

## Indexes - Sorted
	ZADD	timeline:bycreated		[HGET tid:<id> created]		<tid>
	ZADD	timeline:bymodified		[HGET tid:<id> modified]	<tid>
	ZADD	timeline:bytype			[HGET tid:<id> type]		<tid>
