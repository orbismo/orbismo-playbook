# Pet Care

A log of each pet's care: vet visits and what the vet said, vaccinations with the due dates your vet gave, medications and flea, tick, and heartworm preventives exactly as prescribed, weigh-ins, health notes and questions to ask at the next visit, grooming, and who looked after them while you were away. Pets are first-class entities in your world, so "When is Biscuit's rabies shot due?", "What is she on right now?", and "Who looked after the cats last time?" are answered from the records, and a sitter's care sheet is built from them.

**It records; it doesn't advise.** The assistant never suggests a medication or dose, never interprets a symptom, weight, or test result, and never works out a vaccine's due date from general schedules. Due dates come only from your vet, a certificate, a reminder card, or a label, and medication dates from an interval or duration you gave. Clinical questions are pointed back to your vet; if something is happening right now, the first thing you hear is to contact your vet, emergency vet, or an animal poison control line, with any numbers you've saved.

## What it creates

| Entity | Role |
| --- | --- |
| `rule/app_pet_care` | The app itself. Seven lore chunks carry the full spec: Data Model: Pets & the Clinical Line, Data Model: Health Records, Data Model: Providers/Care/Documents, Pets & Vet Care, Medications/Weight/Notes, Care & Life Events, Queries. |
| `interest/pet_care` | The app's hub. It holds the Household Care Sheet: notes for sitters that apply to every pet. |

In use, the app creates `person` entities for pets (`person_type` "pet", tagged `pet-care`); `event` entities for vet visits (past and scheduled), vaccinations, medication courses, weigh-ins, health notes, grooming, and sitter, boarding, or daycare stints; `place` entities for vets, emergency vets, groomers, and kennels; and `reference` entities for certificates, invoices, and other documents. Vets, groomers, sitters, and walkers are `person` entities kept as your ordinary contacts, not tagged as app data.

## Requirements

- Entity types `person`, `event`, `place`, `reference`, and `interest` (Orbismo's common types).
- An owner ("Me") entity, recorded as each pet's owner.
- An app router section in your world instructions (see the [library README](../README.md)).

## Installation

1. Give [app.json](app.json) to your connected AI and ask it to add its entities and relationship to your world.
2. Add this line to the app index in your world instructions (edit them in the Orbismo web UI, or ask your connected AI):

```markdown
- **Pet Care** (`rule/app_pet_care`): triggers: pets and pet care, vet visits and appointments, pet vaccinations and due dates, pet medications and flea/tick/heartworm preventives, pet weight, pet symptoms or health notes, grooming, pet sitters/dog walkers/boarding/daycare, microchips, pet insurance, adopting a pet, a pet going missing, being rehomed, or passing away. Tags: `pet-care`
```

3. Test it: say "We adopted a beagle named Biscuit" and confirm a `person` named Biscuit appears with `person_type` "pet" and a `CARETAKER_OF` link from you with role "owner". Then say "Biscuit got her rabies shot today at Northside Vet, next one due 2027-10-16" and confirm a vet visit at a new Northside Vet place, with a rabies vaccination linked to it whose `next_due` is 2027-10-16.

## Example prompts

Talk about your pets the way you normally would. These show what the app responds to and what happens next.

**At the vet**
- "Took Biscuit to the vet, she has an ear infection, $180. Drops twice a day for 10 days": logs the visit with the vet's words, starts a medication course ending after the 10th day, and asks for the drug's name and dose if you didn't say.
- "Here's Miso's vaccine certificate": reads it, proposes each vaccine with its date and due date exactly as printed, asks about any date it can't read clearly, and saves only what you confirm.
- "Biscuit's dental is November 3 at 9": adds a scheduled visit.
- "Sent the insurance claim for the ear infection": marks the claim submitted on that visit.
- "Vet tomorrow, what should I mention?": lists what you've noted since the last visit, open questions, weigh-ins, and current medications.

**Day to day**
- "Gave Biscuit her NexGard": marks it given today and, if you said it's monthly when you added it, sets the next one due a month later.
- "Biscuit weighed 24.5 kg today": logs a weigh-in. If your vet gave a target weight, it states the difference as a number, never "too heavy".
- "She's been limping since Tuesday": notes it in your words, says to check with your vet, and doesn't guess at causes.
- "Ask the vet about her itching next time": saved as a question for the next visit. After that visit, the assistant asks whether the vet answered it.

**Care while you're away**
- "Sam is watching the cats next week": logs a sitting stint, and asks who Sam is if it isn't clear. Later, "Who looked after the cats last time?"
- "Make a care sheet for Sam": diet, medications with doses as recorded, allergies, routines, vet and emergency vet numbers, and the microchip number, ready to send.
- "The kennel needs proof of vaccination": lists each vaccine's date and due date on file, the microchip number, and active medications, and notes that kennels usually want the clinic's own records.

**Questions**
- "When is Biscuit's rabies due?" · "What's due in the next two months?" · "What is she on?" · "What has Biscuit cost this year?" · "Who's our emergency vet?"

## Notes

- A vaccine's due date is only what your vet, a certificate, or a reminder card gave, never estimated from general schedules, because the right schedule is your vet's call. If none is recorded, the assistant says so.
- Medication end dates and a preventive's next dose are worked out only from a duration or interval you or the label gave ("for 14 days", "monthly"), and the assistant says when a date was worked out that way.
- Doses and frequencies are stored word for word, and the assistant never does dose math. It asks once for a missing or unclear dose; if you don't know it, it stays missing, including on a sitter's care sheet, where it's flagged instead of filled in.
- Asked what a term the vet used means, the assistant gives a one-line general definition, clearly labeled as general. It never says whether a food is recalled or what a kennel, insurer, or country requires; those come from them or your vet.
- When you add a pet, the assistant offers once to save your vet, an emergency vet, and an animal poison control number, so it has numbers to give in an emergency. It never gives a phone number from memory.
- There are no reminders. "What's due?" shows the next 60 days, plus any recorded due date that has passed, when you ask.
- One appointment with several pets becomes one visit per pet, so each pet's history stays its own. A combined bill is split only as the invoice splits it; otherwise the whole bill goes on the first pet's visit, noted as shared.
- Individual doses aren't logged as separate records; each medication keeps the last time it was given and by whom. Daily meals, individual walks, and spending on food and supplies aren't logged; the pet's diet and a walker's schedule are.
- A good-day/bad-day journal is available when you ask to start one, for example in a pet's later years. It reports counts and leaves decisions to you and your vet.
- If someone in your world already has the pet's name, the pet gets its species added: "Biscuit (Dog)".
- Pets that pass away, are rehomed, or go missing keep their whole history. When a pet passes away, the assistant asks before stopping its medications and cancelling scheduled visits, and the pet drops out of "what's due".
- Pets are never added to the [Family Tree](../family-tree/): owners are linked as caretakers, not parents.
- This app logs pet health only. It never records anyone's own symptoms, injuries, medications, or medical care: if a pet bites someone, only the pet's side is logged.
