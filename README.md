# Slow English Learning Videos

An agent workflow that produces a short, family-friendly **3D Pixar-style English story film** inside VideoExpress. You give it an idea; it saves a project and MP4 export in your account with two recurring characters who move, act and speak simple English in every shot. The agent does not download the video.

**Demos:** [sample videos made with this workflow (Google Drive)](https://drive.google.com/drive/folders/1RPgzb6Jhv9ycTm_RgKsJ6qW_WWUCErSc?usp=sharing)

This folder contains only two files:

| File | Purpose |
|---|---|
| `SYSTEM_PROMPT.md` | The complete instruction set for the agent. Paste it as the system prompt of a Claude session that has the Claude-in-Chrome browser tools. |
| `README.md` | This guide. |

## What the workflow does

1. **Intake.** The agent asks three questions in one message: the idea (story, and optionally characters, environments and voices), the ratio (landscape or portrait), and the duration (up to 5 minutes). Anything you leave out, it chooses.
2. **Story and beat plan.** It writes a wish → obstacle → response → payoff story, two character identity records, one fixed voice anchor per character, and a beat map of 4- and 5-second shots that adds up exactly to the requested duration (60 s = 13 beats, 300 s = 65 beats). Every beat has one speaker, one short line, one physical action, one environmental motion and one camera instruction.
3. **Character sheets.** One single-person full-body reference per character, generated as a 3D Pixar-style image with Creative mode on, saved to the library.
4. **Keyframe and clip pairs.** For each beat, create and save one consistent-character keyframe using the saved sheets as references. Immediately enter that beat's video/audio prompt, set and verify its manual video length, and submit Create Video before generating the next beat's keyframe.
5. **Batch checks.** Submit up to five beat videos one by one without waiting for each clip to finish. Their renders may overlap. Once the batch is submitted, wait for all its clips, check their status, duration and source keyframe, then start the next batch. Speech and lip-sync are generated natively from each video/audio prompt; Lipsync HD, text-to-speech, voice cloning and voice changer are not used.
6. **Assembly and export.** For each clip in story order, the agent right-clicks its Media Library tile and chooses **Add to Timeline**. It checks that the clip lands on track 1 before adding the next, then aligns once, saves the project and exports a FullHD MP4. It never drags clips, including during recovery.
7. **Delivery.** The agent reports the finished in-app export title and output ID, a keyframe table, the prompt book, the run state and a candid QA note. It does not download the MP4 or keyframes.

## How to use it

1. Open Chrome, log in to VideoExpress at `https://app.videoexpress.ai/` (production, VideoExpress 3.5; the agent reads your account's library folder ids itself at the start of the run), and make sure the Claude-in-Chrome extension is connected.
2. Start a Claude session with `SYSTEM_PROMPT.md` as the system prompt. The run starts immediately; do not expect a summary or a question about what to build.
3. Answer the single intake message. Example:

   > 1. A boy and his mother find a lost puppy at the market and cycle it home to its owner. Voices: Leo (Son, 10-year-old boy, Australian accent), Sara (Mother, 38-year-old woman, Irish accent).
   > 2. Landscape.
   > 3. 60 seconds.

4. Leave it running. Sheets take about a minute each, keyframes 5–40 s, clips 1–2 min, export about 2 min per 13 clips. A 60-second film finishes in roughly 45 minutes; a 5-minute film takes several hours.
5. Find the finished film under **My Videos** in VideoExpress. Project and export both use the simple story title, with no date or time. Local run directory stores only prompt and state records. If interrupted, say "Resume" and name the run directory; the agent reuses the open generation tab and selected references when available.

## Rules the agent follows

- **One GO approval.** After the three intake answers the agent shows the run plan — 2 character sheets, N keyframes, N clips, one saved project and one export, all on your VideoExpress credits — and waits for you to reply **GO**. After GO it runs continuously: no per-step questions, no handing work back, no stopping at phase boundaries. It stops for a login page, a visible app refusal, an unrecoverable error after retries, an uncontrollable browser, a vanished job, genuine ambiguity, or anything outside the approved plan (deleting saved media, buying credits, new terms).
- **Fast batch validation.** The agent submits up to five keyframe → video pairs, waits for that batch, and checks only job identity, completion, duration and source keyframe. It then continues with the next batch. It does not play or visually QA each clip during production; obvious appearance issues are noted without retries. After all batches it assembles, saves and exports.
- **Voice lock.** Each character's voice anchor (name, role, age, gender, vocal description, accent, "exactly the same voice in every clip") is copied byte-identically into every clip prompt. Since v7 the delivery word after "says" comes from a short neutral list (warmly, gently, calmly, cheerfully, with a smile, …); "shouting", "out of breath", "laughing", "startled" and similar are banned because they change the synthesised voice. Emotion goes into the words and the face instead.
- **Closed cast (v7).** Exactly two characters, both bound to their character sheets. No third person is ever rendered — not a neighbour at the door, not a shopkeeper, not a passer-by. The consistent-character model copies reference clothing onto any unreferenced person (in "The Bakery Delivery" the neighbour appeared in the baker's apron and bandana while the baker lost them). Story roles that need a third party are written off-frame.
- **Wardrobe lock (v7).** One wardrobe string per character is copied byte-for-byte into the character sheet, every keyframe binding ("wears exactly …, nothing added, removed or recoloured") and every clip prompt ("keeps … on and unchanged for the whole clip"); two-shots add "the two characters never share, swap or copy any clothing item". The two wardrobes share no item or colour.
- **Direction and object-integrity locks (v7).** Anything that travels states its screen direction; bicycles add "front wheel leads, moves only forward, never rolls backwards", the rider keeps both hands on the handlebar while riding, and any grab happens in a later beat after a full stop. Vehicles and key props carry a part-count sentence ("exactly one handlebar, one basket, two wheels; no part duplicates, splits, merges or morphs"). Added after tests showed a bicycle riding backwards and a doubled handlebar.
- **Motion rule.** Static talking-head clips are rewritten. Every beat has a locomotion or interaction verb, one environmental motion and one camera instruction, and the prompt ends on a defined framing that keeps the speaker visible.
- **Stability locks.** Prompts state what must not change: props stay where they are (in the basket, in the arms), sizes stay constant, helmets stay on, parked bicycles stay still, exactly one of each animal. These lines were added after the Lost Puppy QA found a vanishing puppy, a removed helmet, a bicycle rolling backwards and a puppy that changed size.
- **Right source frame.** Before Create Video, confirm this beat's selected keyframe id. At batch completion, check the recorded source id or existing library thumbnail for a clear mismatch. Correct a wrong source; do not run per-clip numeric frame comparisons or visual QA during production.
- **Single continuous-shot prompts (v7.3).** Each 4–5 s clip stays in one shot from its accepted keyframe. The prompt names one adult, one child, one owner for each unique prop, one small action and one fixed camera direction. This revision follows a review of *The Garden Basket*, where some keyframes already showed duplicates and other clips gained them during animation.
- **Free hands, solid doors, one of each landmark (v7.2).** Every held object names the holding hand and states that the other hand is empty and visible ("exactly two hands, exactly one phone") — a phone had appeared in both hands and a boy grew a third hand. Anyone who walks gets an open route and a stopping point in front of, never through, doors and furniture; doors open only through a stated hand action. In keyframe prompts each landmark is described once and referred to as "the same clock" afterwards (a clock named twice was rendered twice); signs and windows are described positively as plain and blank (invented lettering appeared despite "no labels"); the child's binding sentence states child proportions and height against the adult (the 11-year-old drifted to toddler proportions); gestures never depend on a finger count.
- **No close-ups, no new objects, one action (v7.1).** Camera sentences come from a short allowed list and must end on a medium shot that keeps the upper body visible; "push-in", "zoom" and "close-up" are banned tokens. Every clip prompt states that nothing may appear that is not already in the keyframe, restates where each prop starts using the keyframe's own words, and asks for one main action; a prop changes hands in at most one clip in three, with a static camera. Added after the same run produced eight close-up endings, a scooter and a gate from nowhere, a duplicated puppy and a puppy that teleported in six clips.
- **Text checks before every submit.** Cast count, wardrobe bytes, delivery list, direction words, integrity sentence, quoted line, anchor bytes, motion verb, camera and audio phrases, word limit. A prompt that fails any check is fixed in the prompt book before anything is generated.
- **One thing per call.** One clip submission per browser script call (45-second limit), a single tab with a Web Worker timer, at most five generations in flight.
- **Run isolation (v7).** Each run keeps an internal `RUN_ID` and separate local state directory. VideoExpress project and export use only a simple story title, with no date or time. The agent checks for title collisions before saving and uses recorded IDs for sheets, clips and the export. The first generation tab stays open with its dialog and character references selected. One other tab may handle monitoring and assembly. No asset gets its own new tab. A resumed agent reuses the same generation tab when available and verifies the reference thumbnails against the run state.

## Browser

The prompt is written for the Claude-in-Chrome extension. If the extension is not connected, the Claude desktop app's built-in browser pane runs the same workflow (verified on two runs); the prompt's §2 lists the small differences.

## Known limits

- Voice consistency across clips comes only from the anchor text and the neutral delivery list; the model can still drift on a line or two. Lip-sync is native and not verified by the agent.
- The locks are prompt text. They make the failures rarer; they do not make them impossible. Visual model failures can remain in the finished film because production checks do not review every frame.
- The agent uses candidate 1 unless an obvious duplicate is visible in the existing result strip, then uses candidate 2 if better. It does not regenerate solely for appearance during production.
- Timings measured on production for a 3-minute (39-beat) film under an earlier all-keyframes-first run: sheets ≈20 s each, keyframes ≈10 min total, clips ≈25 min total, export ≈2.5 min. The interleaved keyframe → clip workflow may take a different wall-clock time; measure the first batch rather than treating those stage totals as a new estimate.
- Portrait export dimensions and a few production-3.5 behaviours are marked UNKNOWN in the prompt and are logged on first observation rather than assumed.
