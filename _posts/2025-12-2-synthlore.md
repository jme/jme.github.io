---
title: synth lore

---


OK, suppose you want to target a different ecosystem, e.g. Rust, but don't have any accumulated knowledge of it. 
Derivative lore can be created by leveraging  the previous [Clojure-based Knowledge Graph](https://github.com/jme/codegen-support-docs/blob/main/simple-webserver/kg.ttl) to seed generation of a Rust language equivalent Knowledge Graph.   

Note that the Clojure Knowledge Graph (*lore*, for short) references specific dependencies e,g, Ring, Compojure, Jetty etc, effectively baking in the bill of materials. The rust ecosystem rendering of this lore will yield inferred equivalents. They could (and normally would) be specified of course but the conceit is that you have minimal rust ecosystem knowledge.

> While 'baking in' those  dependencies seems questionable the KG is a living document and any given reference simply a snapshot of the current nominal form.

**prompt:**
`The attached RDF-based Knowledge Graph (kg.ttl) constitutes an essential  lore for guiding efficient LLM generation of simple web servers, based on the Clojure language ecosystem.`
`Please provide a similar RDF knowledge graph, also in turtle (.ttl) format, but for the Rust language ecosystem`

Which yields a generated [rust (+ Axum + tokio + tower-http + tracing) based lore](https://github.com/jme/codegen-support-docs/blob/main/simple-webserver/derived-rust-kg.ttl) “synthesized” from the Clojure ecosystem version. Since no SBoM was specified we get an inferred stack<sup>*</sup>.

Now all of this may or may not be a good idea. That original lore, while hardly comprehensive, was based on fragments of manual experience. The synthesized version is a guided but still latent space artifact. Over time improvements in the LLM engines may mostly render this caveat moot, but here we are now.
##### and finally, the actual app
Now use this new rusty synthetic lore in combination with a (platform independent) [PRD](https://github.com/jme/codegen-support-docs/blob/main/simple-webserver/prd-simple-webserver-sec.md) to generate the rust app code. If you have minimal knowledge of the rust ecosystem you might not be able to provide an SBoM for the build, so just skip it for now.

**prompt**
`As a very senior software engineer, with over a decade of experience using the Rust language ecosystem, Use the included PRD (prd-simple-webserver-sec.md), in conjunction with the RDF-based comprehensive design, dependency, documentation & guidance lore knowledge graph (derived-rust-kg.ttl) to generate the described web server application, including any associated project artifacts. Afterwards list any task and/or documentation items that were NOT implemented, and the reasons for their omission.`                  

`The PRD shall be considered the Source of Truth for this code generation process; Any conflicts between Knowledge Graph Lore and the PRD shall be resovled in favor of the PRD.` 

`After generating the code, perform a final, rigorous syntax and balance check on all parentheses, brackets, and braces to ensure the code is fully compilable and free of any errant characters.`          

`Keep a running list of each element from the Primary Knowledge Graph (derived-rust-kg.ttl) that is actually used for the generation of the app, along with the inferred semantic requirement that required the usage. Provide this list at the end of the generation process.` 
`Also provide a similar list of the Primary Knowledge Graph elements that WERE used in the generation of the app.`

And...for most reps the resulting [Rust (+ Axum + tokio + tower-http + tracing) basic web server code](https://github.com/jme/codegen-support-docs/blob/main/simple-webserver/rust-server.rs) worked as expected. There were however some concerning version variations of project dependencies, including in at least one case a CVE-bearing deprecated library version 
Obviously it's crucial to keep a close eye on supply chain risks associated with dependency substitutions when -as here- the [Cargo.toml](https://github.com/jme/codegen-support-docs/blob/main/simple-webserver/Cargo.toml) SBoM is a gen artifact; validation & analysis via a Security Role entity is mandatory. Watch the omissions report too.

So yeah, sure of course  you could vibe this new rust app demo all in one step, but it's interesting to watch the hard-won manual domain lore inform the new domain synthetic lore, which then begets the generated app, all guided by the design spec and the meat fingers. 


> A blurry, rapidly evolving shape glances in side-eye; it is the space of LLM agentic codegen capabilities. Its other eye is watching the [synthetic CDO scene from the film "The Big Short"](https://www.youtube.com/watch?v=0X0-NpZpx6U)  
>    

----
*\**  IME LLM codegen can mix-up rust library version tags and actual library functionality, yielding various compilation errors. And this happened in a couple of reps of codegen here. 
Usually these are easy fixes, once you become aware of this particular heuristic failure mode.
But the best 'fix' is something that should be *-and usually is-* done anyway, in manual development or codegen-assisted: a solid software bill of materials for project dependencies.
I've not specified a SBoM for these codegen examples in order to illustrate the codegen issues that can arise. Obviously you would want to avoid doing that in any real projects for a variety of security and sanity reasons.

