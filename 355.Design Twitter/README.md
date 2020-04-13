Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

- postTweet(userId, tweetId): Compose a new tweet.
- getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
- follow(followerId, followeeId): Follower follows a followee.
- unfollow(followerId, followeeId): Follower unfollows a followee.

**Example:**
```
Twitter twitter = new Twitter();

// User 1 posts a new tweet (id = 5).
twitter.postTweet(1, 5);

// User 1's news feed should return a list with 1 tweet id -> [5].
twitter.getNewsFeed(1);

// User 1 follows user 2.
twitter.follow(1, 2);

// User 2 posts a new tweet (id = 6).
twitter.postTweet(2, 6);

// User 1's news feed should return a list with 2 tweet ids -> [6, 5].
// Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.getNewsFeed(1);

// User 1 unfollows user 2.
twitter.unfollow(1, 2);

// User 1's news feed should return a list with 1 tweet id -> [5],
// since user 1 is no longer following user 2.
twitter.getNewsFeed(1);
```

**Solution:**

```golang
type Twitter struct {
	ts   int
	umap map[int]*User
}

type Tweet struct {
	id, time int
	next     *Tweet
}

type User struct {
	id       int
	followed map[int]bool
	head     *Tweet
}

func CreateUser(userId int) *User {
	user := &User{
		id:       userId,
		followed: make(map[int]bool),
	}

	user.follow(userId)
	return user
}

func (this *User) follow(userId int) {
	this.followed[userId] = true
}

func (this *User) unfollow(userId int) {
	if userId != this.id {
		delete(this.followed, userId)
	}
}

func (this *User) post(tweetId, ts int) {
	twt := &Tweet{
		id:   tweetId,
		time: ts,
		next: this.head,
	}
	this.head = twt
}

/** Initialize your data structure here. */
func Constructor() Twitter {
	return Twitter{
		umap: make(map[int]*User),
	}
}

/** Compose a new tweet. */
func (this *Twitter) PostTweet(userId int, tweetId int) {
	if _, ok := this.umap[userId]; !ok {
		this.umap[userId] = CreateUser(userId)
	}
	this.umap[userId].post(tweetId, this.ts)
	this.ts++
}

/** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
func (this *Twitter) GetNewsFeed(userId int) []int {
	if _, ok := this.umap[userId]; !ok {
		return nil
	}
	users := this.umap[userId].followed
	mh := CreateMaxHeap(len(users))

	for id := range users {
		tw := this.umap[id].head
		if tw != nil {
			mh.Add(tw)
		}
	}

	var res []int
	for mh.Size() != 0 {
		if len(res) == 10 {
			break
		}
		tw := mh.PopMin()
		res = append(res, tw.id)
		if tw.next != nil {
			mh.Add(tw.next)
		}
	}

	return res
}

/** Follower follows a followee. If the operation is invalid, it should be a no-op. */
func (this *Twitter) Follow(followerId int, followeeId int) {
	if _, ok := this.umap[followerId]; !ok {
		this.umap[followerId] = CreateUser(followerId)
	}
	if _, ok := this.umap[followeeId]; !ok {
		this.umap[followeeId] = CreateUser(followeeId)
	}
	this.umap[followerId].follow(followeeId)
}

/** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
func (this *Twitter) Unfollow(followerId int, followeeId int) {
	if u, ok := this.umap[followerId]; ok {
		u.unfollow(followeeId)
	}
}

type MaxHeap struct {
	data  []*Tweet
	count int
}

func CreateMaxHeap(capacity int) *MaxHeap {
	return &MaxHeap{
		data:  make([]*Tweet, capacity),
		count: 0,
	}
}

func (this *MaxHeap) Add(tw *Tweet) {
	this.data[this.count] = tw
	this.count++
	this.shiftUp(this.count - 1)
}

func (this *MaxHeap) PopMin() *Tweet {
	if this.count == 0 {
		panic("this heap is empty")
	}

	ret := this.data[0]
	this.data[0] = this.data[this.count-1]
	this.count--
	this.shiftDown(0)

	return ret
}

func (this *MaxHeap) Size() int {
	return this.count
}

func (this *MaxHeap) shiftUp(i int) {
	for i > 0 && this.data[i].time > this.data[(i-1)/2].time {
		pi := (i - 1) / 2
		this.data[i], this.data[pi] = this.data[pi], this.data[i]
		i = pi
	}
}

func (this *MaxHeap) shiftDown(i int) {
	for 2*i+1 < this.count {
		j := 2*i + 1
		if j+1 < this.count && this.data[j+1].time > this.data[j].time {
			j++
		}

		if this.data[i].time >= this.data[j].time {
			break
		}
		this.data[i], this.data[j] = this.data[j], this.data[i]
		i = j
	}
}

/**
* Your Twitter object will be instantiated and called as such:
* obj := Constructor();
* obj.PostTweet(userId,tweetId);
* param_2 := obj.GetNewsFeed(userId);
* obj.Follow(followerId,followeeId);
* obj.Unfollow(followerId,followeeId);
 */
```
