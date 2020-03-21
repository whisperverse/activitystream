# ActivityStream 🏞

## Welcome to the Fediverse


This is a personal experiment to explore Activity Streams.  This library is BRAND NEW, and is not ready for use **by anyone**, for **any reason**, at **any time**.  Check out https://github.com/go-fed for a library that's ready for prime time.


# OMFG

ActivityStreams is so hard to work with in a "strongly typed", "idiomatic go" way, because the W3C spec is so loose with types.  While this is *great* for a Javascript, or old ColdFusion programmer, it's super-cumbersome to try to squeeze this into a Go, or TypeScript paradigm.

So, here's the idea (for now).  EVERYTHING is *stored* as a `map[string]interface{}`, which is as big of a no-no as using Reflect.  But, it's the only way to support the entire spec without doing [this](https://github.com/go-fed/activity/blob/master/streams/vocab/gen_type_activitystreams_accept_interface.go).

Consistency will be "enforced" (or at least *encouraged*) using constructor functions that can construct these data structures in a "proper" way, and validator functions that can accept (nearly) any input, and then format it in a consistent manner.

Something like:

```go

// WRITING ActivityStreams

record := as.Object{
    "actor": as.Object{
        "name": "John Connor",
    },
    "object": as.Object{
        "content": "hey-oh",
    },
    "target": as.
}

record := activity.Add(
    actor.Person().Name("John Connor")),   // actor
    object.Note().Content("Hey-oh"),        // object
    object.Collection().Name("John's Blog") // target
)

return as.New(
    as.Actor().Name("John Doe").ID("john@doe.com").URL("john.doe.com"),
    as.Subject().URL("SomethingHere.com"),
    as.Target().ID("Some.Kind.Of.Target")
)



// READING ActivityStreams

activityStreamParser := NewParser()

Parser.CustomField("x:customField", func(interface{}) (interface{}, error) {
	// Parse custom field into any other data structure, and return
	// it, along with an error (if needed)
})

// This function will parse the data into a structure.  It works
// like json.Unmarshal, but uses default and custom field definitions
// to re-parse fields from all of the crazy formats into a consistent
// USABLE format for all data elements.
Parser.Parse(input []byte) (result map[string]interface{}, err error)

```

## Pull Requests Welcome

This library is a work in progress, and will benefit from your experience reports, use cases, and contributions.  If you have an idea for making Rosetta better, send in a pull request.  We're all in this together! 🏞