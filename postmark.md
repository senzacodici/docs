---
description: Sample exercises
---

# Postmark

## Exercise 1

> * A customer reached out to Postmark support asking about a feature we don’t currently have \(and it's not coming up in the near future\). How would you handle that and what would you reply to them with?
>   * Please also tell us a little about why you answered this way. What is your thought process behind it?

Hello John,

Regarding your question, I'm sorry to inform you that we don't currently support the requested feature in Postmark. 

Even though this feature is not planned in our current roadmap, I'm going to share your feedback with our product team, so they can be aware of it, and keep it in mind for future improvements in the Postmark platform.

In any case, let me check with the developers if there's any workaround to achieve a similar result with our current solution, until such feature becomes available. I'll reply back as soon as I have more information about it.

Let me know if you have further questions.

Have a nice day, best regards  
Damian

> * Please also tell us a little about why you answered this way. What is your thought process behind it?

I tried to be honest about the availability of such feature, avoiding excuses or false expectations. I also wanted to share this feature request with the Product team, since requests from users and customers can be \(sometimes\) a great resource for ideas and future developments. I also wanted to find a solution or provide a workaround to the customer, so I committed to contact the dev team and see if there's any alternative solution available.

## Exercise 2

> * Explain to a customer how to authenticate their domain with DKIM
>   * Use our documentation in [http://postmarkapp.com/support/](https://postmarkapp.com/support) as a resource.

Hello Jane,

To authenticate your `postexample.com` domain with DKIM, you'll need to add a DKIM record on your Postmark web settings, and copy that record to your DNS provider. Once the record has been added to your DNS servers, Postmark will automatically verify it within 48 hours.

Now let me guide you trough these tasks in detail:

**Adding the DKIM record**

1. Login to Postmark and select your domain, `postexample.com`
2. Navigate to your **Sender Signatures** tab.
3. Under the domain you are adding DKIM authentication for, click **DNS Settings** or **Add a DKIM DNS record**.
4. A DKIM record for `postexample.com` will be shown on your screen.
5. Copy the DKIM information, you'll need it to complete the next task.

**Setting the DKIM record in your DNS**

1. Login to your host or DNS provider and access the area where you can add new DNS records.
2. In your DNS manager, add a new TXT record, using the `Hostname` and `Value` you copied in the previous step.
3. Save the changes, and allow your DNS provider to update the records.

**Verify the DKIM record**

No further action is required on your side, as Postmark should verify the updated record within 48 hours. If you do not see the record automatically become verified, you can use the **Verify** button to initiate a manual verification check from Postmark's web interface.

 You can find a video guide, and additional screenshots regarding DKIM setup in this article: [https://postmarkapp.com/support/article/1091-how-do-i-set-up-dkim-for-postmark](https://postmarkapp.com/support/article/1091-how-do-i-set-up-dkim-for-postmark)

You can also find additional help regarding the DNS setup with popular providers such as CloudFlare, DreamHost, and GoDaddy in this link: [https://postmarkapp.com/support/article/1090-resources-for-adding-dkim-and-return-path-records-to-dns-for-common-hosts-and-dns-providers](https://postmarkapp.com/support/article/1090-resources-for-adding-dkim-and-return-path-records-to-dns-for-common-hosts-and-dns-providers)

Please let me know if you had any issues, or need further assistance to set up DKIM on your domain.

Have a nice day, best regards  
Damian

