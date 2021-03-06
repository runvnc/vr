# Packaging Named Data Networking, Procedural Generation, the Metaverse, and the Semantic Web Together

The more that I think about it, the more I think that we actually do want something like description logics (Manchester OWL syntax is the nicest representation I have seen) as an underlying format for this metaverse protocol.  Because I am trying to create a general purpose (representation-neutral) format for the geometry generation code, and a simple outline in YAML or something doesn't really cut it.  Also, when will the semantic web/linked data/DL be integrated into the metaverse if we don't do it at the beginning?

One note about why I am including procedural generation.  Its just an incredibly powerful technique for creating infinitely scalable and extremely detailed virtual worlds that have extremely efficient compressed representations (in the form of algorithms) that can go over the wire very quickly.

So in order to make this an attractive system that people will want to invest in, store DL facts and rules on the NDN network that represent the virtual reality world and its processes.  So now when I go to open up my Metaverse/VR browser, I can type in some particular NDN node, or I can do a semantic query.  When I go to a particular place on the web, I can click on a 3d representation of a product and the whole transaction can be processed automatically by machine agents that operate on a dynamic market enabled by that common DL-based information exchange.

Basically the idea is that having amazing procedurally generated 3d worlds to visit on a decentralized scalable network will help motivate people to make the investment in learning and implementing DL (or maybe some other semantic web system) widely.  So all of the advantages of each system reinforce each other.

To make it a really attractive proposition, make the whole thing free.  In other words, create reference systems compatible with a variety of platforms and languages that are completely open source (MIT/Apache etc.).

# Simple Metaverse Protocol Proposal

This is a related idea that doesn't involve the semantic web or DL.

After seeing the level to which http://lucidscape.com/ has gone, I would like to present an alternative view.

Their proposal seems to be about maximizing things like performance and massive scale in terms of objects across servers.  I'm not sure we need to start by emphasizing those things. Maybe we don't need to have the most advanced open source 3d networking game engine out there as a protocol.  Maybe we just need something simple enoughto implement that has some key features of the metaverse and allows for clients to communicate.

So my idea uses an emerging standard (NDN) for efficiently distributing information and updates about the metaverse, an easy to read standard (YAML) for describing the details, includes the concept of portals linking between worlds (you gotta have portals, they are one of the coolest things about the metaverse), includes the idea of embedded webviews (too much awesome stuff on the web to leave that out of the metaverse browser), a YAML-format for encoding algorithms that can be easily translated to common languages and compiled on the fly, and updates for informing other clients about operations on the world state.


Maybe we can start by building off of Named Data Networking (NDN) http://named-data.net/doc/NDN-TLV/current/

Would be nice to have everything including content represented in Manchester OWL syntax but that's probably not realistic.

So perhaps use the NDN protocol with TLV encoding, NDN name format/URI scheme, order. Use the interest packets for subscribing to rooms in the metaverse. Then exchange NDN Data Packets with updates to object positions etc.

XML is a bit redundant so I propose that we use YAML.

```yaml
    name/smith/john50/homeworld

    base: org/sim
    libs: dim/1, objs/1, phys/1
      dimension: real
        - terrain:
            heightmap: maps/hills.png
        - objects:
          - house:
            windows: 5
            type: simple
            bedrooms: 3
          - avatar:
            model: models/man.dae
            name: name/smith/john50/me
            pose: walk 2
            position: [50, 50, 2]
            velocity: [0.5, 1.0, 0.1]
            owner: name/smith/john50
          - portal:
              world: name/wallace/liz10/homeworld
              type: simplecircle
              position: [123, 435, 123]
              dimension: real
          - webview:
              name: John's Smart TV
              url: http://en.wikipedia.org/wiki/Metaverse
              position: [100, 400, 50]


  org/sim/objs/1

  base: org/sim
  dimension: real
  procedures:
    room:
      - for: 
          range: [w, 1, walls]
          block:
            - makegeom: [cube, 1.0, 0.5, 1.0, 0.0, 0.0, 0.0]          
    house:
      - for: 
          range: [b, 1, bedrooms]
          block: 
          - room:
            walls: 4
      - for: 
          range: [b, 1, windows]
          block:
          - cutgeom: [quad, 0.25, 0.25, 0.25, b, 0, 0]  
  
         
  name/smith/john50/update102
  
  timestamp: xxxxxxxxxxxx
  which: name/smith/john50/me
  update:
    acceleration: [ 0.25, 1.0, 0.0 ]
    velocity: [ 0.5, 0.0, 0.0 ]
    position: [ 13, 123, 0 ]
```

The idea with the "procedures:" thing is to take advantage of YAML's flexibility in order to create a language independent representation of an algorithm for doing simple procedural generation. It would be designed in such a way as to make it easy to generate code from the data tree outputting to different languages like C, C++, Nim(rod), Java, Python, GLSL, whatever. That would be compiled on the fly and loaded as a sort of plugin, or simply run through an interpreter (compiling would execute faster obviously).  This way we don't force anyone to embed a particular scripting language (or code their whole application with it).  My example shows for loops with code blocks that contain function calls.

The assumption is that we don't really need to have massive scale and a perfectly synchronized phyics engine.  In most cases it will be good enough for a client to send an update about the new position and velocity of its own avatar when the user does a control action.  Then clients can do their own physics simulation (if they want).  In some cases you will really want to have some kind of conflict resolution for things like synchronizing collision outcomes, but this is not really necessary in order to get most of the benefits of the metaverse, and so does not need to be part of the core protocol at the start.



