## Discord.js Tutorial - a short tutorial
After this tutorial you are able to code a cool bot with many commands and a level system.
### Setup

Did you ever want to write your own bot but you think you are too bad for it or you don't have motivation to do so? Stop thinking that way.
Writing discord bots using discord.js is really, really, easy if you know some javascript basics.
	
In this tutorial we will use some ES6, which makes javascript more powerful.
If you have never heard of ES6, I'll explain some stuff whenever I use it.
Found a mistake? Just comment. 

Discord.js is a node.js package which requires at least Node.js v8.
If you don't know your node.js version, execute `node -v` in any command line program.
I'm not going to explain how to install node.js, because you can just google it.
Node.js allows you to run <b>serverside</b> javascript code and the node package manager (npm) provides more than one million packages, free to use.

Enough about node.js, let's get into it.
To install the discord.js package, just run `npm i discord.js` (or `npm i discord.js --save` if you want to write it automatically in your package.json file) <b> in your bots' directory.</b>
<b>Important for windows users: </b>Normally cmd opens in system32, so navigate into your directory using cd (otherwise it'll install that package in system32).
Just ignore those warnings, they're not important to us.
If installed, create a file - name doesn't matter, I recommend index.js or something that is similar to that and open it with your text editor.

### Your first lines of code
Node.js has a method for implementing packages or files: `require();`
As I mentioned before, ES6 offers much more stuff which makes JS better, for example more datatypes: `const` and `let`.
If you create a const, you won't be able to modify its value anymore. It is read-only.
So let's create a new const for `require();` (since we want to make sure that we don't modify it by mistake.)
```js
const Discord = require("discord.js"); // Requires the npm package 'discord.js'.
```
If you ever programmed in another language like C++, C#, Java, etc., you probably know classes. If not, don't worry.
We need the `Client` class of the npm package to interact with the Discord api.
That's why we create an instance of Discord.Client; let's call that instance `client`.
```js
const Discord = require("discord.js");
const client = new Discord.Client(); // Create an instance of Discord#Client
```
Now we can login with our application token using `client.login()`.

Token? What? What token? How do I get one?
Go to the discord developer page, login, create an application, create a bot user and copy the token.

If you got your token, call `client.token()` with your token as parameter.
```js
const Discord = require("discord.js");
const client = new Discord.Client(); 

client.login("Your Token");
```
> <b> Warning! </b>
> Keep your token PRIVATE. It is just like a password.

<b> Congrats! </b> You wrote your first bot.
So we want to test if it works and if our bot goes online.
Open your command prompt, navigate into bots' folder and type  `node <filename>`. (`<filename>` being the filename of your file that you've created some seconds ago).
If nothing is in your console, it works. 

Cool! Our bot is online, but ... how can we invite our bot to our server?
It's actually really simple. Just generate an OAuth2 url on the developer page.

If your bot joined the server, you should see that your bot is online.

### Events and handling messages 
Some node.js packagss have events. But, what is an event?
An “EventEmitter“ takes two parameters: the event <b>type</b> and a function that will be evaluated when the event gets fired.
Discord.js has many of these; our most important event is `message`. It gets fired <b> every time </b> a message is being sent.
Here's the syntax for these Event Emitters:
```js
client.on(type, function);
```
> PLEASE don't attach an event emitter for each command. Just use if-statements!
	
So now you know what events are and what they do. Let's make one to interact with incoming messages.
```js
client.on("message", (message) => {

});
```
Oooh, what is going on there? 
Let me explain: as I said, `client` has an event of type `message`, so we use `"message"` as first parameter. 
The second parameter looks a bit weird if you've never used ES6.
Those things are called “arrow functions“.
There's actually no difference between
```js
function(foo){
	console.log(foo);
}
```
and
```js
(foo) => { console.log(foo); }
```
You can use the first one if you don't like the second one, I'm using it <strike>because programmers are lazy</strike>.
(Do note that arrow functions don't have a bound `this` context)
The parameter `message` is an object with many properties.
The most important properties for now are:

`message.content` The message content
`message.author` The <b>user</b> object of message author - more later
`message.member` The <b>guildmember</b> object of message author (yes, there is a BIG difference between `author` and `member`)
`message.id` The id of the message
`message.channel` The channel object where the message was sent in
`message.guild` The guild object where the message was sent in (null if message is in a dm channel)
(Check out the <a href="https://discord.js.org">docs</a> for a list of all properties of each class

### First command: ping
So we know about eventemitters and other stuff, let's make our first command.
First of all, we check if the content of the message equals to `!ping`.
If that condition is true, the bot should send a message with `pong!`.
Here's how it works:
```js
// client is an instance of Discord.Client
client.on("message", (message) => {
if(message.content == "!ping"){ // Check if content of message is "!ping"
		message.channel.send("pong!"); // Call .send() on the channel object the message was sent in
	}
});
```
So, as the name says, `.send()` sends a message to a text channel.
You can <b>only</b> call `.send()` on a text channel (otherwise it'll throw an error and your bot crashes)
> By the way: Calling `.send()` on user objects such as `message.author` will send a dm to the user.
> That's how you send private messages.

Congrats another time! You successfully made your first command.

### Editing a message
But what if you want to edit a message or delete one? What if you want to pin a message?
> You can only edit your own messages, it doesn't matter if you have permissions to manage messages.
To edit a message, you call `.edit()` on a message object with the new message content as parameter:
```js
message.edit("This is an edit");
```

### Resolving promises
If you want to send a message and edit it, you have to resolve the promise that `.send()` returns (or using an asynchronous function)
It's uncommon that a method which sends a request doesn't return a promise.
Editing it's own message could look like this:
```js
message.channel.send("Hello").then((newMessage) => {newMessage.edit("Edited!");});
```
To resolve a promise, call `.then` on it. That method takes a function as parameter and I used an arrow function.
On the docs you can see what a method returns (and want parameter the promise returns, `.send()` for example returns the message object)
> If you use an asynchronous function, you can await that promise.

### Other message methods
Guess, there's a `.delete()` method which deletes the message you call `.delete()` on.
Normally you have to use `setTimeout` to wait some time, but the delete method offers a timeout parameter which waits an amount of time.
Pinning a message is easy too, just call `.pin()` on the message - and `.unpin()` unpins a message.
Another cool thing you can do is adding reactions on messages. Simply call `.react()` on the message object. As parameter you use a unicode emoji or a custom emote id.
```js
message.react(":thinking:"); // No
message.react("🤔"); // Yes
message.react("123456789"); // If it's a valid emote id, yes
```

### A real ping command
Let's make a "real" ping command which displays how long it took to send the message
Since the message object has a 'createdTimestamp' property - which returns the seconds since 1/1/1970 00:00 AM -, we can substract the current timestamp with message.createdTimestamp
```js
client.on("message", (message)){ // EventEmitter
	if(message.content == "!ping"){ // Check if message is "!ping"
			message.channel.send("Pinging ...") // Placeholder for pinging ... 
			.then((msg) => { // Resolve promise
				msg.edit("Ping: " + (Date.now() - msg.createdTimestamp)) // Edits message with current timestamp minus timestamp of message
			});
		}
}
```
This is really neat, isn't it? But there is much more stuff that I want to show you.

### Difference between GuildMember / User Objects
As I said before, there's a huge difference between them.
You can <u>only</u> call/read guild-related methods/properties from the member object (message.member)
User related stuff like tag, discriminator, avatar has to be called/read on a user object (message.author)
Examples:
```js
message.author.ban(); //💣Cannot ban an user
message.member.ban(); //🆗

message.author.tag; //🆗
message.member.tag; //💣 GuildMember objects doesn't have a tag property

message.author.addRole("123456789"); //💣 Cannot add role to a user
message.member.addRole("123456789"); //🆗
```

### Roles
Of course Discord.js can add roles to members, edit roles and more stuff.
GuildMember objects have a `.roles` property, which returns a <b>Collection</b> (more about collections later) with all roles the member has.
To check if a member has a specific role, you use the `.roles` property and call `.has(<Role id here>)` on that collection. `.has()` requires a <b>snowflake</b> (it actually takes the key of the element that we used on the extended map, but since they are keyed by ID...) as parameter.
Here's an example to check if a member has a role and if they do, send "Hi"
```js
// let's say there's a role on the guild with id 123456789
if(message.member.roles.has("123456789")){ // Check if member has role
	message.channel.send("Hi"); // Send "Hi" if they do
}
```

You can create a role by calling `.createRole()` on a guild object.
That method takes an <b>object</b> as parameter. To give that role a name, use the `name` property. 
Also you can set role colors with the `color` property.
A short example on creating a role in current guild
```js
message.guild.createRole({ // Call createRole on guild object
	name: "Cool role",
	color: "#ff0000" // ColorResolvable; either "RANDOM", hex as string or color name
	permissions: [ // Array of permissions the role should have 
		"SEND_MESSAGES",
	],
	hoist: true, // Whether the role should be displayed on the right side (online list)
	position: 0 // Role position
});
```
Also there are other properties for creating a role.
By the way, Guild#createRole returns a promise, means you can give that role to a member after creating it

To give a role to a member, call `.addRole` on a guildmember object. You give 

### Collections
You may have heard of collections (respectively maps) and if you've never heard of them, don't worry. I'm going to explain what collections are.
client has a property guilds. This property is a collection. There is actually no big difference between collections and maps (collection extends map).
Collections are like Maps, but have specific methods which makes it easy to get objects from it.
If you still have no clue what a map is or how they're built.
To get a specific object from a map you can call `.get()` on it. That method takes one parameter: the key
> The key of the object is the "title" of the object
there are much more methods for maps.
Example of mapping:
```js
let names = client.guilds.map((u) => { return u.name });

names; // ["Some name", "Other one"];
```
We have to know what maps are for the next section.

### Get guild, user, etc. by ID
Nice, now we know how to send a message to the channel the message was sent in, but what if we want to send a message to a channel in another guild?
If you remember, client.guilds returns a collection of guilds the client has access to and since it's an extended map you can call `.get()` on it to get a specific object from it.
If you didn't get it, here is an example:
```js
client.guilds.get("GuildID"); // returns the guild object with id you provided as parameter of .get())
```
After calling .get() on it, you can read any property of it that the guild class has (roles, members, ...)
Then, read property `channels` of that guild - which returns a collection of channels the guild has - and call `.get()` on it again and provide the channel id as parameter.
```js
client.guilds.get("GuildID").channels.get("ChannelID").send("Message from an other channel: " + message.content);
```

As I said before, you can call `.get()` on ANY collection. (docs show if something returns a collection)
Also you can check if something is in a collection, for example checking if the message member has a specific role:
```js
let member = message.member; // Making a variable and storing the message authors' member object in it
if(member.roles.has("123456789")){ // Checks if message member has a role with id "123456789"
	message.channel.send("Cool role you have there!");
}
```

It's easy, isn't it?

### Catching promise rejections
Sometimes something goes wrong and we normally catch errors with try-catches, but you can't catch <b>Promise Rejections</b> with these try-catches (unless the scope is async).
For now (node.js v8.9.4) it will catch it but it's <b>deprecated</b>, means that your process will exit with an edit code that isn't 0 in the future if you continue not catching them.

Catching promises is actually really easy: Just append a `.catch()` on the Promise.
That method takes a function as parameter. If that Promise gets rejected, e.g. a Discord API Error, it'll evaluate the code in your function.
Let me give you an example:
```js
try {
	message.channel.send(""); // empty message; will fail
}catch(e){
	console.log(e);
}
/* Proper way */
message.channel.send("").catch((e) => { console.log(e); });
```
I personally don't like that way of catching, but that's how it has to be done. ¯\_(ツ)_/¯ 


### Taking arguments - user input
One of the coolest things bots can have are user inputs, like letting the user append something on a command: `!ban @User`, you know?
As you already know, `message.content` returns a <b>string</b> with the message content.
We can split that string by `" "` using `message.content.split(" ")`. That returns an array.
```js
// Imagine message.content is "!ban @User because he is annoying"
let array = message.content.split(" ");

array; // ["!ban", "@User", "because", "he", "is", "annoying"]
```

So what we could do to get everything after "!ban @User", is splitting the string into an array by whitespace, slicing the array (removes an index) and join that array again to a string.
```js
// Imagine message.content is "!ban @User because he is annoying"
message.content.split(" ").slice(2).join(" "); // because he is annoying
```

### Bulk deleting (as bot)
Every second or third bot has a `clear` command, which clears an amount of messages. But, how is it done? Is it deleting x messages in a channel manually?
No, there's a method for it: `TextChannel#bulkDelete`. That method takes one argument: The amount of message to delete.
> You can only delete up to 100 messages at one and try not to delete messages older than 14 days.
```js
message.channel.bulkDelete(100); // Deletes 100 messages in channel the message was sent in.
```
> bulkDelete doesn't work on user accounts. In the next section we'll discuss how you do it with selfbots.


### Bulk deleting (as selfbot)
**Note: This no longer works in Discord.js v12+**
> Warning! Using a selfbot is against the terms of services. 
> Use them on your own risk!
> I've warned you.

As I said in the previous part, `bulkDelete` only works with bot accounts. 
There's no official method for mass deleting on <b>user</b> accounts, you have to do it with hacky code.
`TextChannel#fetchMessages()` fetches an amount of messages* and returns a promise with fetched messages.
After you've fetched an amount of messages, you call `.forEach()` on the collection and call `.delete()` on every message object.
> Please don't do that 1000 times a minute, else it's API abuse and there's a good chance that youd get banned.
Here's an example on how it works:
```js
message.channel.fetchMessages({ 
	limit: 50 // Fetch last 50 messages.
}).then((msgCollection) => { // Resolve promise
	msgCollection.forEach((msg) => { // forEach on message collection
		msg.delete(); // Delete each message
	})
});
```

* `TextChannel#messages` is only a collection with <b>cached</b> messages, means you could run into bugs if you try to get a message from it.
	
	
### Custom welcome messages
What about custom welcome messages? Let"s make one.
Discord.js provides an event called `guildMemberAdd`. That gets fired every time a user joins a guild.
It requires one parameter: The guildmember object.
So let's create one:
```js
client.on("guildMemberAdd", (member) => { // EventEmitter, nothing new

});
```
Cool thing, but how can we send a message to the main channel?
Well, some months ago, `Guild.defaultChannel` became deprecated and you can't use it anymore.
But with some tricks we can do it.
There's a property called 'systemChannel' on Guild. It is the channel where the discord welcome messages will be sent in.
> `Guild#systemChannel` is either a textchannel object or null.
But sometimes that property is null, so we have to check if it's falsy before doing something with it.
You might wonder how we can get the guild object when the parameter is a guildmember object? GuildMember have a `.guild` property which returns the guild object.
```js
client.on("guildMemberAdd", (member) => {
let guild = member.guild; // Reading property `guild` of guildmember object.
let memberTag = member.user.tag; // GuildMembers don't have a tag property, read property user of guildmember to get the user object from it
if(guild.systemChannel){ // Checking if it's not null
	guild.systemChannel.send(memberTag + " has joined!");
}
});
```
Really cool, isn't it?
> There's a `guildMemberRemove` event which emitts if a user leaves a guild

### Embeds
Did you see these messages with a colored border on the left side?
These things are called 'embeds'. Discord.js has an own class for RichEmbeds.
To create an instance of it, you call the constructor: `new Discord.RichEmbed()`
> You can use an object instead of the richembed constructor, too.
You can either put the created instance in a `.send()` method or in a variable.
But (obviously) that contructor has some awesome and useful methods to build the embed.
> https://discord.js.org/#/docs/main/stable/class/RichEmbed
Let's create an embed for the welcome messages.
```js
client.on("guildMemberAdd", (member) => { // Check out previous chapter for information about this event
let guild = member.guild; 
let memberTag = member.user.tag; 
if(guild.systemChannel){
	guild.systemChannel.send(new Discord.RichEmbed() // Creating instance of Discord.RichEmbed
	.setTitle("A new user joined") // Calling method setTitle on constructor. 
	.setDescription(memberTag + " has joined the guild") // Setting embed description
	.setThumbnail(member.user.displayAvatarURL) // The image on the top right; method requires an url, not a path to file!
	.addField("Members now", member.guild.memberCount) // Adds a field; First parameter is the title and the second is the value.
	.setTimestamp() // Sets a timestamp at the end of the embed
	);
}
});
```

### Bonus: Use a JSON file for token and prefix
Do you want to have a custom file where variables are stored? I'll show you how.
The key is `require()`. It let's us implement - as mentioned at the beginning of the tutorial - npm packages and other js/json files.
If you want to require js files and call/read properties from it, you have to export it.
After you've required the json file, you can read properties from it.

Our json file, located in `./config.json`
```json
{
   "prefix": "!",
   "token": "MzVVCFGBDKabJgA.GwhJCa.UwbbHa"
}
```
Main file:
```js
const config = require("./config.json");
client.login(config.token); // Uses value of key 'token' in config file.
```

### Bonus: Commands in different files
Everythings fine, but all commands in one file isn't very beautiful.
Let's make it a bit more cleaner.
Let's create an other file for a command: `ping.js`
Paste your command in that file and export it using `module.exports`
Syntax for exporting a module is:
```js
module.exports = function;
```
(You can export variables with `module.exports.variableName` too.)

In our example it shall look like this:
```js
module.exports = (message) => { // Function with 'message' parameter
	message.channel.send("Pong!").catch(e => console.log(e));
}
```
So you might wonder why we need a parameter in that function. 
If we wouldn't do that, it'll throw an error, because there's no variable declared as 'message'.
```js
module.exports = () => {
   message.channel.send("Pong!"); // message is not defined
}
```
In our main file we will call that module and use `message` as parameter, so it is defined for that scope.

After exporting the module, we can require it in our main file:
```js
const ping = require("./ping.js"); // Requiring module ping.js

client.on("message", (message) => {
	if(message.content == "!ping"){
			ping(message);
		}
});
```
 Notice the `./` in `require()`? It means that we want a file in the <b>current directory</b>. If we wouldn't use a `./`, it would search for a module in `node_modules`.

### Bonus: Levelsystem with JSON
> I'd recommend using SQL for a levelsystem due to corruptions and the way fs works, but we want to keep it simple.

The FS module comes with node.js, means we don't have to install another package.
First of all, we want to make sure that bots don't get an entry in our file.
Simply put a `if(message.author.bot) return;` at the beginning of your message event. 
It is reading property `bot` of message.author which returns a boolean - whether the author is a bot. And if it's a bot, return. Everything after `return` wont be evaluated.
Create a `.json` file - name isn't important and put `{}` in it (so it's a blank object).
Now we want to require it to read/write to it.
It is automatically formatted as an object, so you don't have to stringify it.
```js
const scores = require("./scores.json");
typeof scores; // object
```

So let's create an object for the message author if there is no.
```js
if(!scores[message.author.tag]){ // Check if there's an object called as message authors' tag
	scores[message.author.tag] = { // Create a new object in scores variable
		money: 0 // New property "money" in user object
	};
}
```
> Make sure that `message` is defined.
Now, we wan't to increase the number of points by 25 every time a message was sent.
```js
scores[message.author.tag].money += 25; // Increase by 25
```

After that, we want to write the new object into the json file.
FS provided a method called `writeFileSync()`, which writes something to a file <b>synchronously</b>.
Syntax for that is:
```js
fs.writeFileSync(path, content[, options]);
```
	
Soo, at the end it would look like:
```js
if(message.author.bot) return;
if(!scores[message.author.tag]){ 
	scores[message.author.id] = { 
		money: 0 
	};
}
scores[message.author.tag].money += 25;
fs.writeFileSync("./scores.json", JSON.stringify(scores));
```
> JSON.stringify() stringifies an object. If we don't stringify it, the file would be `[object Object]` since it implicitly calls toString on the object.
