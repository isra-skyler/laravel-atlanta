
# Utilizing HAL and JSON:API Standards for Hypermedia APIs with Node.js
As an API architect focused on designing maintainable and discoverable services, I'm constantly challenging myself to learn new techniques. A recent project at [Hybrid Web Agency](https://hybridwebagency.com/) pushed me well outside my typical work - creating a hypermedia-driven Node.js API from the ground up.

I set an ambitious yet practical goal - fully implement the HATEOAS style according to the HAL and JSON:API specifications through rigorous testing. This would provide hands-on experience applying emerging web standards in a real-world context.

Over six months, I developed a fully-featured order management system that served responses as hyperlinked state representations. Evaluating how each spec structured relationships between order, product, and customer resources revealed important contrasts in their approaches.

In this article, I'll discuss the key insights uncovered. Code samples demonstrate the nuances of modeling interconnected resources. Ultimately, sharing learnings from this journey aims to benefit fellow API designers exploring RESTful architecture design.

It's through ongoing challenges like this that I expand my expertise in crafting elegant yet robust APIs. My dedication to learning serves both my development and ability to deliver sophisticated solutions for clients.







### What is HATEOAS?
HATEOAS (Hypertext As The Engine Of Application State) defines a RESTful design paradigm where hypermedia guides clients through an API. Rather than requiring out-of-band knowledge about endpoints and relationships, HATEOAS APIs embed necessary instructions within responses.

By including links in data payloads, APIs following HATEOAS enable clients to dynamically discover valid next interactions. Links teach where to navigate after each request, removing dependencies on predefined sequences. For instance, retrieving an order resource may reveal links for associated products, customers or invoices.

This allows clients flexible traversal of resources as opposed to static endpoint configurations. Evolving the API structure through new entities or relations poses minimal disruption, as pre-existing clients simply follow hypermedia prompts.

Adopting the HATEOAS approach dissociates client implementations from understanding how resources interconnect within the API. Well-designed HATEOAS interfaces ensure responses drive further engagement through provided affordances.

Changes such as enhancing payloads with additional links then propagate naturally to all clients. Over time, APIs comporting with this style organically cultivate discoverability, resiliency and independence from any preconceptions of their domain model. All through utilizing linked data as the governing engine of client-server dialogues.


### Benefits of HATEOAS

By building upon hypermedia as the driver of self-description, HATEOAS-style REST APIs realize several key advantages:

**Self-Documenting Interactions** - Each response implicitly outlines its own interactive context through embedded links, eliminating separate specification artifacts.

**Fluid Evolution** - The API remains decoupled from clients and can organically grow by revealing new relations or refactoring endpoints over time.

**Resiliency to Change** - Modifying the domain model through structural changes is transparently accommodated by all clients simply following updated links. 

**Progressive Exposure** - Hypermedia responses allow unveiling expanded functionality post-release through an inside-out development mindset.

**Exploratory Interfaces** - Clients can discover overlooked interactions by dynamically traversing relational prompts within responses.

**Adaptive Navigation** - HATEOAS frees clients to reactively prioritize resource flows tailored to contextual needs with each request.

Fundamentally, the hypermedia-driven design lends REST interfaces inherent evolution, teaching, long-term viability and flexibility - properties upheld through an emphasis on responses guiding their own exploration and progress over time.


## Choosing a Hypermedia Approach

When implementing HATEOAS in a Node API, two commonly used specifications are Hypertext Application Language (HAL) and JSON:API. 

### HAL

The HAL specification defines a simple format using a top-level "_links" property to represent relationships between resources.

   ````json
{
  "_links": {
    "self": { "href": "/orders/1" },
    "items": { "href": "/orders/1/items"} 
  }
}
   ````
   
HAL is lightweight, intuitive, and gets the job done. However, it only supports basic hypermedia needs.

### JSON:API

JSON:API establishes a full JSON API standard, explicitly connecting resources through a "relationships" object containing related IDs and links.

   ````
   "relationships": {
     "items": {
       "links": {
         "related": "/orders/1/relationships/items"  
       },
       "data": [...ids]
     }
   }
   ````
   
It defines a richer hypermedia format but with more complex implementation than HAL.

### Choosing an Approach

For simple use cases, HAL is sufficient due to its minimal overhead. JSON:API fits more robust APIs by providing a full specification framework. Consider the intended hypermedia needs and API design to determine the right level of standardization. Both enable HATEOAS principles.



## Developing a Node.js API with Hypermedia Links

This guide demonstrates how to build a RESTful API that implements the Hypertext Application Language (HAL) specification using Express.

### Incorporating Required Modules

The `express-hal-builder` library is installed to generate HAL compliant representations of resources and their relationships.

```
npm install express-hal-builder
```

### Modeling application resources 

Schemas are defined for Order and OrderItem using JavaScript object structures.

```js
const Order = {};
const OrderItem = {};
```

### Generating Hypermedia Representations

The HAL builder enables mapping entities to hypermedia by:

1. Representing a resource entity 

2. Embedding links to associated child resources

```js
app.get('/orders/:id', (req, res) => {

  const order = //...

  const builder = new HalBuilder();

  builder.representEntity(order)
    .childLink(order.items, '/orders/'+id+'/items');

  res.json(builder.get());

});
```

This provides the framework to develop a REST API that follows HAL specifications by rendering resources and their relationships through hypermedia links.



## Discovering Resources in a HAL API

Making requests to uncover the API interface and navigate between linked entities.

- Send initial GET requests to expose starting points
- Parse responses to identify relation types  
- Traverse connections by following URIs in _links 
- Gradually reveal the data model through exploration

## Building a Linked API with JSON:API

This guide demonstrates how to develop a RESTful backend that represents relationships between resources as specified in the JSON:API standard using Express.

### Adding Dependencies

The `jsonapi-serializer` module helps generate standardized responses.

### Modeling Core Entity Types

Schemas define the structure of resources like Order and Item.

### Creating JSON:API Documents 

Responses relate data across entities by:

1. Serializing a parent resource

2. Including references to associated children

```js
app.get('/orders/:id', async (req, res) => {

  const order = await Order.findById(req.id);

  res.json(order.serialize({
    include: 'items'  
  }));

});
```

This lays the foundation for a compliant JSON:API implementation connecting resources through standardized linkage.

# Conclusion

In summary, applying established API specifications provides structure, discoverability and future-proofs applications against changing demands. Aligning with standards ensures services remain intuitive.

Developing complex, interconnected backends per specifications requires crossing many technical disciplines. Partnering with a proficient Node.js vendor can streamline this.

As a full-service Node.js development company located in Atlanta, we offer [Node.js Development Services in Atlanta](https://hybridwebagency.com/atlanta-ga/custom-laravel-development-services/) to assist with building robust, specification-driven APIs. Our engineers have deep experience implementing HAL, JSON:API and other norms for relating resources. Whether prototyping, scaling or full life cycle efforts, we aim to deliver production-ready REST services on time and on budget.

By capitalizing on our Node.js and framework know-how plus architectural best practices, teams can prioritize core work while we ensure technical execution meets quality and performance standards. Contact us to discuss how our Atlanta-based services could expedite your project goals.
## References

- https://www.halspec.org/
The official specification website for HAL (Hypertext Application Language), detailing the standard.

- https://jsonapi.org/
The official website for the JSON:API specification, including documentation on compliant API design. 

- https://expressjs.com/en/advanced/best-practice-performance.html
Best practices guide from the Express.js website covering performance, security, and standard compliance for Node.js APIs.  

- https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design
Documentation from Microsoft on API design best practices, including modeling relationships and using standards.

- https://swagger.io/resources/building-blocks/hypermedia/
Swagger overview of hypermedia APIs and industry standards like HAL and JSON:API for enabling discoverability.
