tokenbucket
====================

This package provides an implementation of [Token bucket](https://en.wikipedia.org/wiki/Token_bucket) scheduling in Go. It is useful for implementing rate-limiting, traffic shaping, or other sorts of scheduling that depend on bandwidth constraints.


Example
==============


To create a new bucket, specify a capacity (how many tokens can be stored "in the bank"), and a rate (how often a new token is added).

````go

    // Create a new bucket
	// Allow a new action every 5 seconds, with a maximum of 3 "in the bank"
	bucket := tokenbucket.NewBucket(3, 5 * time.Second)
````

This bucket should be shared between any functions that share the same constraints. (These functions may or may not run in separate goroutines).


Anytime a regulated action is performed, spend a token.

````go
	// To perform a regulated action, we must spend a token
	// RegulatedAction will not be performed until the bucket contains enough tokens
	<-bucket.SpendToken(1)
	RegulatedAction()
````

`SpendToken` returns immediately. Reading from the channel that it returns will block until the action has "permission" to continue (ie, until there are enough tokens in the bucket).


(The channel that `SpendToken` returns is of type `error`. For now, the value will always be `nil`, so it can be ignored.)



