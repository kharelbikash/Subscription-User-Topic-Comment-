# Subscription-User-Topic-Comment-(Bulletin Board):
Implementation of Bulletin Board in Elixir. Erlang VM which allows parallel processing and distributed Mechanism. 

A bulletin board consists of a list of topics t1,t2,...,tk
and users that can subscribe to specific topics and post new information about
a topic. Newly posted information is forwarded to all currently subscribed users.
The goal is to implement a distributed bulletin board system that supports the
following operations:
subscribe(ui,tj): subscribes user ui to topic tj . This ensures that user ui will
be notified (by receiving a message) whenever an update is posted for topic
tj in the future. If the topic tj does not exist yet, it is created with user ui
as the (only) subscribed user.
unsubscribe(ui,tj): unsubscribes user ui from topic tj such that ui is no longer
notified of updates to the topic.
post(ui,tj,content): user ui posts content regarding topic tj . This causes no-
tification messages (attaching content) to be sent to all currently subscribed
users.
Your implementation should adhere to the following guidelines:
 Initially, no user is subscribed to any topic.
.For each topic tj , a topic manager is responsible for handling the subscription
of users and implementing the broadcast of newly posted information to the
current subscribers. To ensure fault-tolerance, a topic manager is replicated
on all available nodes (described below). It is recommended that you imple-
ment this functionality in a module TopicManager. To be discoverable by
users, the topic manager for topic tj registers its process ID (PID) under the
topic name in the process registry.
Q.Write a module User that provides an API to facilitate the interaction of
users with the topic managers. The module should include the functions
subscribe(topic name,user name), unsubscribe(topic name,user name),
post(user name,topic name,content), and fetch news(). A new user joins
the system by executing User.start(user name), which registers the user's
name (with this PID) in the global process registry.
Example:
User.subscribe("Alice","computing")
:timer.sleep(5000)
IO.puts("Updates to Alice's subscriptions: #{User.fetch_news()}")
Meanwhile, Bob runs the following code:
User.subscribe("Bob","computing")
User.post("Bob","computing","Just found a new prime number.")
User.unsubscribe("Bob","computing")

When Alice's process resumes its operation and executes User.fetch news(),
she will receive the message previously posted by Bob, which was relayed to
her by the topic manager of "computing".
