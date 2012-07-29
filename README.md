PlayJongo Play 2.0 Module
=====================================

This is a Play 2.0 Module for Jongo http://jongo.org/ 
(a MongoDB Java driver wrapper)

Installation
-----------

The project is currently released to a github repository so you will need to add the following to your projects Build.scala

	val appDependencies = Seq(
	  "uk.co.panaxiom" %% "play-jongo" % "0.2"
	)
	
	val main = PlayProject(appName, appVersion, appDependencies, mainLang = JAVA).settings(
      resolvers += ("GitHub Play Repository" at "http://alexanderjarvis.github.com/releases/")
    )
    
You can also override the application.conf configuration to specify your MongoDB host, port and DB:

	# Play Jongo
	# ~~~~~
	playjongo.host="127.0.0.1"
	playjongo.port=27017
	playjongo.db="play"


Usage
-----

To use this module you just use the PlayJongo class which manages your Mongo and Jongo instances for you. It provides the same method calls as the Jongo object as detailed in the Jongo documentation: http://jongo.org/ .

A simple example:

	public class User {
	
		public static MongoCollection users() {
			return PlayJongo.getCollection("users");
		}
		
		@JsonProperty("_id")
		public ObjectId id;
		
		public String name;
		
		public User insert() {
			users().save(this);
		}
		
		public void remove() {
			users().remove(this.id);
		}
		
		public static User findByName(String name) {
			return users().findOne("{name: #}", name).as(User.class);
		}
		
	}