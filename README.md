# twarc2 🐍🐦💾

twarc2 is a *proposed* new version of [twarc] custom made for the new
Twitter v2 API. Twitter [substantially changed] their API in July 2020.
They have deprecated the v1.1 and premium endpoints. While some of the API
calls seem similar the most significant change is the response payload with
is [totally new]. The representation of a tweet now can take a very
different shape depending on what you ask for and how you ask for it. 

As with twarc the goal of twarc2 is to make it easy to continue to get the
fullest representation of tweets and Twitter users, with the minimum amount
of fuss, and maximum amount of reliability. Here's a sketch of what the
command line API could look like.

### Search

    twarc2 search blacklivesmatter > tweets.jsonl

If you have academic search turned on for your account:

    twarc2 search blacklivesmatter --all > tweets.jsonl

### Filter

The [filter] API works a bit different in v2 because you first create a set
of rules (optionally tagged) and then you connect to the filter stream to
retrieve all the tweets that match those rules.

    twarc add-rule blacklivesmatter
    twarc list-rules
    twarc remove-rule 1
    twarc filter > tweets.jsonl

### Sample

    twarc2 sample > tweets.jsonl

### Followers

    twarc2 followers jack > users.jsonl

### Following/Friends

    twarc2 friends jack > users.jsonl

### Users

    twarc2 users ids.txt > users.jsonl
    twarc2 users usernames.txt > users.jsonl

### Timeline

    twarc timeline jack > tweets.jsonl

### Mentions

    twarc mentions jack > tweets.jsonl

### Hydrate

    twarc hydrate ids.txt > tweets.jsonl

### Compliance

The [compliance] API is new and allows users to upload tweet id datasets
and get back information about whether the tweets are still available.
Since this can take some time for large datasets the ids are first uploaded
and then fetched.

    twarc add-compliance ids.txt
    twarc list-compliance
    twarc get-compliance id

### Stitching

The big difference with the v2 API is that some information (users, media,
etc) is not included inline and can be included as a separate [extended
entities] object. This cuts down on duplication of information in
a payload, but it also means your output stream will contain individual
tweets followed by an entities object.

If you would like these entities to replace the id references in tweets you
can use the `--stitch` option. Depending on your use case this could make
downstream processing easier since it can expect each line to contain
a tweet.

    twarc search blacklivesmatter --stitch > tweets.jsonl

[twarc]: https://github.com/docnow/twarc
[substantially changed]: https://blog.twitter.com/developer/en_us/topics/tools/2020/introducing_new_twitter_api.html
[totally new]: https://blog.twitter.com/developer/en_us/topics/tips/2020/understanding-the-new-tweet-payload.html
[filter]: https://developer.twitter.com/en/docs/twitter-api/tweets/filtered-stream/introduction
[compliance]: https://developer.twitter.com/en/docs/twitter-api/tweets/compliance/introduction
[extended entities]: https://developer.twitter.com/en/docs/twitter-api/v1/data-dictionary/object-model/extended-entities
