# n8n-AI-room-agent

**Hotel Room Assignment AI**  (n8n + Apaleo)



This project is an **AI-powered hotel room assignment assistant** built in \[n8n](https://n8n.io/).  

It integrates with the **Apaleo API** to automatically assign (or upgrade) hotel rooms based on guest history, reservation details, and room availability.



✨ **Features:**

\- Uses Google Gemini AI as a reasoning agent.

\- Checks guest booking history for upgrades.

\- Considers birthdays for complimentary upgrades.

\- Avoids downgrades and ensures only available/clean rooms are assigned.

\- Capacity-aware: avoids upgrading if the upgraded category has fewer than 5 rooms left.

\- Fallback logic: keeps original unit if no valid upgrade is available.



---

&nbsp;🔄 **Workflow Overview**



1\. **Trigger**: Manual execution or chat message.

2\. **Reservation** **Fetch**: Pulls guest reservation, history, unit groups, and available units from Apaleo.

3\. **AI Agent**: Applies hotel-specific upgrade rules.

4\. **Validation (JS)**: Parses AI output into JSON and applies fallback logic if needed.

5\. **If Node:**  

&nbsp;  - ✅ True: Assigns upgraded room via Apaleo API.  

&nbsp;  - ❌ False: Keeps original assigned room.  

6\. **Apaleo Reservation Action**: Final API call to confirm the unit assignment.



---



&nbsp;🤖 **AI Agent Rules**



\- If guest is new (0 previous bookings) → upgrade.

\- If guest has 5+ bookings → upgrade.

\- If stay overlaps with guest’s **birthday** → upgrade.

\- Only assign rooms that:

&nbsp; - Are in allowed `status` (Available, Ready, CheckedOut).

&nbsp; - Have `condition = Clean`.

&nbsp; - Are not occupied.

\- Do not downgrade guests.

\- If multiple upgrades are possible → choose the closest higher-ranked group.

\- If fewer than 5 upgraded rooms are left → do not upgrade.
