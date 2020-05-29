# WordPress

> Briefly explain “decoupled WordPress”, as you would to a tech-adjacent middle manager who asks you this at a networking event.

A traditional CMS has two essential components: a frontend and a backend. The frontend is the presentation layer that distributes the content to a web page or application, and the backend is where the content is managed and stored. Since these two layers cannot be easily separated, the system is described as tightly coupled.

In a "headless" CMS, the frontend is removed, leaving only the backend. This allows for the content to be published anywhere because the backend is agnostic of the presentation layer.

A decoupled WordPress, having a frontend and backend, is like a traditional CMS but separated \(hence the name decoupled\), allowing the backend repository to deliver content headlessly if needed. This solution combines the flexibility and adaptability of a headless CMS and the user-friendliness of a traditional CMS. Yet, unlike traditional CMS, frontend and backend components are decoupled and communicate with each other through standard API calls. This gives developers the ability to push content to websites, smartwatches, and any other technology that comes out while allowing content creators to use well-known and easy-to-use tools for creating and publishing content without any technical assistance.

> A client has a launch date 2 business days from now. We haven’t received their import yet, and won’t complete it in time for the scheduled launch. What would you do in this situation?

Case assumptions: 

* there's a defined launch plan \(data migration is a part of it\)
* the import tasks have been defined and tested in advance 
* the client has assigned a stakeholder to the project
*  there's a sales team representative involved with this new client

In such a situation, I'll first try to contact the assigned client stakeholder, let him know that we are currently in a deadlock with the migration, and ask him if we can have a quick call to discuss the issue and the possible solutions.

In the call, I'll explain that we haven't received the required data to start the migration and that this issue will have an impact on the planned schedule. I'll let him know that we have a few options to move forward with the project:

* Postpone the launch date, defining a new date to receive the required import data, and committing to an additional follow-up call to avoid missing the launch date again.
* Since the import tasks have been defined and tested previously, as a workaround we could do a partial import \(for example using only the last 6 months of data\) and have it ready in time for the scheduled launch, provided that they send the partial data as soon as possible.

After the call, I'll contact internally the Automattic sales team representative assigned to this customer, and let him know the updated schedule and any other decision made during the call. This should allow the sales team to answer any inquiry from the client's business team regarding the launch.

> Pick a process that you’re excited or interested in — professionally or personally. Examples might be: how to launch a new feature on a website; how to install a development tool; how to fix a flat tire; or how to cook a favorite meal. How would you explain the purpose of this process and the process itself to a new client unfamiliar with the process? Explain who your audience is, and any assumptions about them that may be relevant to your explanation.



