# Sample Call

An illustrative example of a standard booking.

---

**Agent:** Thanks for calling! This is Emma. How can I help you today?

**Customer:** Hi, my water heater stopped working. No hot water since this morning.

**Agent:** Sorry to hear that. No hot water is no fun. Let's get a technician out to you. Can I get your full name?

**Customer:** Mike Turner.

**Agent:** Thanks, Mike. What's the best number to reach you?

**Customer:** 555-310-2290.

**Agent:** That's 5-5-5, 3-1-0, 2-2-9-0. Correct?

**Customer:** Yep.

**Agent:** And what's the service address, including the ZIP code?

**Customer:** 88 Pine Avenue, 30301.

**Agent:** Great, we cover that area. What day works best for you?

**Customer:** Thursday, if possible.

*Agent calls `check_availability` → n8n reads Thursday's dispatch calendar*

**Agent:** On Thursday I have 8 to 10 AM, 12 to 2 PM, or 4 to 6 PM. Which works best?

**Customer:** Noon to two.

*Agent calls `book_appointment` → urgency `standard`, service `water_heater`*

**Agent:** You're all set, Mike. Your technician will arrive between 12 and 2 PM on Thursday, and they'll call 30 minutes before. There's a {{SERVICE_CALL_FEE}} diagnostic fee, which goes toward the repair if you go ahead. You'll get a text confirmation in a moment. Anything else I can help with?

**Customer:** No, that's great. Thanks!

**Agent:** Thanks for choosing us, Mike. Have a great day!

---

## What happened behind the scenes

1. **`check_availability`:** n8n pulled Thursday's events from the dispatch calendar, removed the windows already booked (10–12 and 2–4), and returned the three open ones.
2. **`book_appointment`:** n8n created job `JOB-…`, added a row to the **Jobs** sheet, and put the visit on the dispatch calendar (`WATER HEATER - Mike Turner`, with the address as location).
3. Mike got a text:
   > Hi Mike, your water heater visit is booked for 2030-03-14, arrival 12 PM to 2 PM. Job #JOB-MVSH5HXO. Your technician will call 30 min before arriving. Reply STOP to opt out.
4. n8n returned the job number to Vapi, and the agent confirmed the booking.

If Mike had reported a burst pipe, the agent would have walked him through shutting off the main water valve first, booked it as an `emergency`, and the on-call technician would have received an SMS straight away.
