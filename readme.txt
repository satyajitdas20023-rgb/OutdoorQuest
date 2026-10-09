# WildQuest — Mobile Frontend Implementation Prompt

## 1. Objective

Act as a senior mobile UI/UX designer and React Native engineer.

Build **WildQuest**, a polished, minimalist biodiversity-exploration mobile app. Users discover species, take photographs, record wildlife sounds, complete weekly quests, earn points and build a permanent collection of collectible wildlife cards.

Your task is to implement the **actual frontend**, not just produce mockups, a design proposal or static screens.

The frontend must be fully interactive and use realistic mock data where real services are unavailable.

## 2. Non-Negotiable Rules

Follow these rules throughout implementation:

1. **Mobile only.** The target application is a phone-focused, native-feeling mobile app using Expo, React Native and TypeScript. Do not build or optimise a website, desktop dashboard or web-first interface.
2. **Inspect before editing.** Examine the existing repository, framework, folder structure, dependencies, navigation, assets and existing functionality before changing files.
3. **Protect existing code.** Reuse existing components, models and services wherever possible. Do not delete working features, replace the project unnecessarily or overwrite unrelated files.
4. **Frontend only.** Do not modify Flask/Python backend code, API endpoints, server-side AI integrations, environment files or backend configuration.
5. **No real AI integration yet.** Species identification, bird-call identification, habitat recommendations and other unavailable external services must use clearly identified mock services.
6. **No fake live data.** Do not present mock sightings, coordinates, distances, biological facts or identification results as verified information.
7. **Use real device capabilities.** Users must be able to take photographs inside the app using the camera, select images from the gallery, and record and play back bird-call audio.
8. **Implement, don't just plan.** Inspect the code, make the necessary changes, run the available checks and fix errors introduced by your changes.

### Project structure rule

- If an Expo React Native app already exists, work in that app.
- If the repository contains only the backend and no suitable mobile frontend, create a dedicated `mobile/` Expo React Native application without modifying the backend.
- If another frontend exists but is not an Expo React Native app, do not delete or rewrite it. Create the mobile app separately rather than performing an uncontrolled framework migration.
- Never create a second mobile app if a suitable Expo app already exists.

Before editing, determine which situation applies and follow the corresponding rule.

## 3. Design Direction

Create a premium, minimalist nature-exploration experience inspired by a naturalist's journal, an elegant wildlife field guide and a collectible species album.

The interface should feel organic, calm, sophisticated and rewarding. Prioritise beautiful wildlife photography, generous whitespace, clear typography, subtle dividers and consistent spacing.

Avoid generic AI-dashboard layouts, childish cartoon graphics, flashy gaming effects, excessive shadows, excessive rounded containers, glassmorphism and bright blue-purple gradients.

### Colour palette

Create reusable design tokens for these colours:

| Name | Hex |
|---|---|
| Dark Walnut | `#582f0e` |
| Saddle Brown | `#7f4f24` |
| Toffee Brown | `#936639` |
| Camel | `#a68a64` |
| Khaki Beige | `#b6ad90` |
| Dry Sage | `#c2c5aa` |
| Sage Green | `#a4ac86` |
| Dusty Olive | `#656d4a` |
| Ebony | `#414833` |
| Charcoal Brown | `#333d29` |

Use warm cream as the main background, off-white for cards, Ebony or Charcoal Brown for primary text, and muted sage and olive for secondary elements and progress indicators.

Use Toffee Brown or Saddle Brown as restrained rarity accents. Do not imply that rarity means endangered status.

Use a clean, readable sans-serif font. A subtle serif display font may be used for major headings if a suitable font is already available. Do not install unnecessary font packages.

Maintain sufficient colour contrast, safe-area handling, readable text in daylight and comfortable touch targets.

## 4. Main Navigation

Implement five main destinations in a mobile-native bottom navigation bar:

1. **Home** — weekly progress and recommended discoveries.
2. **Quests** — quest modes, difficulties and active challenges.
3. **Explore** — species and suitable habitats.
4. **Collection** — permanently collected species cards.
5. **Community** — leaderboard and shared discoveries.

Profile and Settings should be accessible from the Home header or a dedicated profile screen.

Every navigation item must open a functioning screen. Provide clear entry points for taking a photograph and recording a bird call.

## 5. Home Screen

Create an editorial-style Home screen, not a dense dashboard.

Include:
- A welcoming greeting and discreet location label.
- Current weekly points and selected difficulty.
- Weekly progress indicator.
- Active quest card with a Continue Quest action.
- Photograph Discovery and Record Bird Call shortcuts.
- One or two recommended species.
- A short walking-distance summary using demonstration data where necessary.
- A horizontal strip of recently collected species.
- Links to Explore and the full Collection.

Do not overwhelm the screen with statistics. More detailed information belongs on dedicated screens.

## 6. Configurable Quest Modes

Implement four initial modes:

### Wildlife Explorer
Discover plants, birds, insects and other wild animals. Support bird-call recording where appropriate.

### Fishing Mode
Discover and photograph freshwater and coastal fish. Allow suitable evidence types to be configured separately from wildlife observation tasks.

### Beach Mode
Discover coastal species such as crabs, starfish and sand hoppers. Include relevant habitat categories such as sandy shores, rocky shores, tidal pools and estuaries.

### Zoo Mode
Browse a selected demonstration zoo, explore its animals and collect zoo observations linked to that zoo or exhibit.

Each mode should have an attractive photographic card, description, relevant habitats, supported evidence types, available difficulties, quest progress and a Start or Continue action.

Implement a central, typed **quest-mode configuration registry**. Adding a mode such as Forest Trek, Wetland Explorer, Rock Pool Explorer or Night Insect Hunt must not require rewriting the navigation system or the shared quest-progress engine.

Different modes must be allowed to support different species categories, evidence types, objectives and safety guidance. Do not assume every mode requires bird audio, plants or the same type of observation.

Zoo discoveries must remain distinct from wild sightings.

## 7. Weekly Quests and Points System

The main game mechanic is **earning points from accepted, distinct species discoveries**.

Do not use fixed species-count requirements for Easy, Medium or Hard. Difficulty is based on configurable weekly point targets.

### Initial local-rarity point values

| Local rarity | Points |
|---|---:|
| Very common | 5 |
| Common | 10 |
| Uncommon | 20 |
| Rare | 35 |
| Very rare | 50 |

### Initial weekly difficulty targets

| Difficulty | Target |
|---|---:|
| Easy | 50 points |
| Medium | 150 points |
| Hard | 300 points |

Store these values in a central configuration or scoring service. Never scatter scoring values or progression rules through individual screen components.

The thresholds must be configurable globally and optionally per quest mode.

Provide difficulty selection, current points, target points, remaining points, a progress indicator, recommended targets and quest completion states.

Easy should favour readily accessible species. Medium should encourage broader discoveries, while Hard should suggest more challenging but realistic discoveries.

Use clearly labelled mock rarity and point values. Actual location- and season-dependent rarity calculations will be implemented by the backend later.

### Duplicate handling

Keep these two concepts separate:

- **Permanent collection:** One unique species card per user and relevant discovery category. Further accepted observations of that species belong in its observation history.
- **Weekly scoring:** Award the full unique-discovery reward only once per species within the applicable weekly quest and scoring category. Repeated submissions must not repeatedly award points.

Zoo and wild observations should be scored separately where the mode configuration requires it.

Support Pending, Accepted, Uncertain and Rejected observation states. A predicted identification must not automatically earn points.

Keep collection history persistent even when weekly quest progress resets.

## 8. Species Identification: Camera and Gallery

This is a mandatory functional feature.

Provide a dedicated identification flow with these steps:

1. Select Plant, Bird, Insect, Fish or Other Animal.
2. Choose Take Photo or Select from Gallery.
3. For Take Photo, open an actual in-app camera interface using a package compatible with the installed Expo SDK.
4. Capture a photograph and preview it.
5. Allow the user to retake, discard or submit the image.
6. Pass the image to a replaceable mock identification service.
7. Display the simulated result.
8. Allow the user to retry or save an observation to the Collection.

Use suitable, currently supported Expo camera and image-picker modules. Inspect the installed SDK before adding dependencies.

Handle denied permissions, unavailable hardware, camera errors and gallery errors gracefully. A permission denial must not crash the app.

### Mock identification results

Display:
- The selected photograph.
- Predicted common and scientific names.
- A demonstration confidence score.
- Alternative candidate species.
- Available habitat and description information.
- Estimated local rarity and points.
- Save Discovery and Try Again actions.

Include an uncertain-identification state. Explain when an image is insufficient for reliable identification.

All predictions and confidence scores must be labelled as demonstration results. Do not call an actual AI service.

The user's own captured or selected photograph must be retained as the observation image and shown on the resulting species card.

## 9. Bird-Call Recording

Build a dedicated in-app audio-recording flow using the phone microphone.

Include:
- Microphone permission handling.
- Record and Stop controls.
- Recording duration and recording status.
- Playback.
- Discard and Submit actions.
- A mock identification result with an uncertainty state.

Use an audio module compatible with the installed Expo SDK. Do not blindly install an obsolete package or replace unrelated dependencies.

Submitting should create a mock observation only through the appropriate service flow.

Display a reminder not to approach nests or disturb wildlife to obtain recordings.

Do not implement real bird-recognition models or backend integration.

## 10. Permanent Species Collection

The Collection is a core feature, not a temporary weekly leaderboard.

Every accepted unique discovery must become a collectible wildlife field card and remain available after weekly quests reset and after the app restarts.

### Card appearance

The user's photograph must be the main visual focus. Each card should include:
- Photograph.
- Common name.
- Scientific name.
- Species category.
- Local rarity.
- Points awarded.
- Discovery date.

Use understated borders and earthy rarity accents. Do not use flashy, animated trading-card effects.

For the demonstration collection, use suitable placeholder images only where real user photographs are unavailable. Clearly label demonstration entries.

### Collection screen

Include:
- A photographic grid and, if practical, a list-view option.
- Search by common and scientific name.
- Filters for plants, birds, insects, fish, other animals and zoo discoveries.
- Sorting by newest, rarity and points.
- Total unique species.
- Total accepted points.
- A compact collection-progress summary.
- A useful empty state.

Tapping a card must open the species-detail screen.

### Species details

Display the user's photograph, names, category, local rarity, points, discovery date and observation history.

Display relevant biological information where available:
- Animals and birds: typical weight, lifespan, habitat and geographic distribution.
- Fish: aquatic habitat, size or weight where available.
- Insects: life cycle, typical size and habitat.
- Plants: growth form, typical height, life cycle and habitat.
- Zoo observations: zoo and exhibit details where available.

Do not show identical or irrelevant fields for every category.

Never invent biological statistics. For mock content, label sample information as demonstration data. Otherwise, omit unavailable values or display "Information unavailable".

Distinguish local rarity from global conservation status.

## 11. Explore and Habitat Recommendations

Create a dedicated Explore screen containing:
- A location selector and search bar.
- Category and habitat filters.
- Map/List toggle.
- Suitable public locations and habitats.
- Recommended species with rarity, points and a short recommendation rationale.
- A Where Should I Look? action.
- Estimated encounter suitability where useful.

Example recommendation explanations include:
- "Suitable coastal habitat nearby."
- "Previously recorded in the wider region."
- "Suitable wetland habitat for this species."

These are demonstration examples, not verified local results.

Use a mock recommendation service that can accept location, species category, habitat, season and suitability information. If actual maps or location services are unavailable, provide an attractive demonstration map and a functional list fallback.

Clearly distinguish historical observations, estimated habitat suitability and mock data. Never invent live sightings, verified distances or confirmed species presence.

If location permission is denied, allow the user to select a demonstration or manually selected location.

Satellite imagery, live biodiversity APIs, production routing and real environmental analysis are out of scope.

Prefer safe, accessible public places. Do not encourage trespassing, disturbing wildlife or pursuing dangerous or sensitive species.

## 12. Community and Leaderboard

Create a clean community interface rather than a flashy esports leaderboard.

Include:
- Weekly rank.
- Avatar and display name.
- Weekly points.
- Unique species discovered.
- Walking distance, when available.
- Recent discovery photographs.
- Filters for points, species discovered and distance.
- Community discovery cards and related species information.

Use consistent demonstration data. Clearly identify mock users and observations.

Do not expose precise home locations or live routes. Provide privacy controls for public photographs and optional statistics.

## 13. Profile and Settings

Include:
- Display name and avatar.
- Lifetime unique species discovered.
- Accepted discovery points.
- Quests completed and weekly streak.
- Walking distance.
- Achievement badges.
- Recently discovered species.
- Past weekly challenges.
- Privacy and application settings.

Allow optional public statistics to be hidden.

Use working local preference controls for settings that are represented in this frontend prototype.

## 14. Frontend Architecture

Keep the application modular and ready for future Flask REST API integration.

Create or reuse typed interfaces for:
- `UserProfile`
- `Species`
- `SpeciesObservation`
- `QuestMode`
- `QuestTemplate`
- `QuestTarget`
- `QuestProgress`
- `HabitatRecommendation`
- `LeaderboardEntry`
- `ZooProfile`
- `Achievement`
- `EvidenceType`

Use reusable components where appropriate, such as:
- `QuestModeCard`
- `DifficultySelector`
- `QuestProgressSummary`
- `QuestTargetCard`
- `SpeciesCard`
- `SpeciesEvidenceCard`
- `HabitatRecommendationCard`
- `ObservationGallery`

Keep these responsibilities separate:

**Screens:** rendering, navigation and user interactions.

**Configuration:** quest modes, difficulty thresholds, rarity values and evidence requirements.

**Services:** identification, audio identification, collection management, quest progress, habitat recommendations, leaderboard and zoo data.

**Persistence:** local demonstration progress, collection entries and user preferences.

Use a central mock-data layer and replaceable service interfaces. Do not bury scoring, duplicate detection or quest rules inside screen components.

Reuse the existing project's state management and storage conventions if suitable. Otherwise, use an appropriate lightweight local persistence solution compatible with the installed Expo SDK.

Persist collection entries independently of weekly quest progress. Persist only the local demonstration data needed for this prototype.

Keep API keys out of the frontend. Do not introduce a production database, real authentication or new backend services.

## 15. Functional Requirements

Do not deliver a collection of static screens. Implement the main interactions end to end.

Verify that the user can:

1. Navigate between all five main destinations.
2. Browse all four quest modes and select difficulty.
3. Start and continue a quest.
4. Submit a mock accepted observation and update quest progress consistently.
5. Avoid repeat rewards for duplicate submissions.
6. Capture an image with the in-app camera and select an image from the gallery.
7. Preview, retake, discard and submit an image.
8. View mock identification results and uncertainty states.
9. Record, stop, play back, discard and submit an audio recording.
10. Save a species to the Collection and open its details.
11. Search, filter and sort the Collection.
12. Add repeat observations without creating duplicate unique-species cards.
13. Browse Explore recommendations and switch between map and list presentation.
14. Browse the leaderboard, community cards and Zoo Mode.
15. Change profile and privacy settings.
16. Close and reopen the app without losing persisted demonstration collection data.
17. Reset or roll over weekly progress without deleting lifetime discoveries.

Keep point totals, unique-species counts, collection entries and quest progress internally consistent.

Provide coherent loading, empty, permission-denied, uncertain-result and error states.

## 16. Scope Restrictions

Do not implement any of the following:
- Backend development or Flask modifications.
- Real AI APIs or model training.
- Live biodiversity data integrations.
- Satellite processing or analysis.
- Production authentication or a production database.
- Fake verified sightings or fabricated live statistics.
- Unnecessary framework migrations.
- Unnecessary dependencies or unrelated refactoring.

Mock the unavailable services behind clean interfaces so they can be connected to the actual backend later.

## 17. Implementation Order

Work through these stages in order:

**Stage 1 — Inspect and protect.** Identify the actual app structure, frontend entry point, navigation, Expo SDK, dependencies and existing working behaviour. Decide whether to modify the existing Expo app or create a separate `mobile/` app according to the project-structure rules above.

**Stage 2 — Foundation.** Implement or refine design tokens, shared components, navigation, TypeScript interfaces, mock fixtures and service boundaries.

**Stage 3 — Core experience.** Implement Home, Quests, difficulty selection, all four quest modes, configurable scoring, Collection and species details. Ensure data consistency and local persistence.

**Stage 4 — Device interactions.** Implement in-app photo capture, gallery selection, image preview, mock identification and the complete bird-call recording flow.

**Stage 5 — Supporting screens.** Implement Explore, habitat recommendations, Community, leaderboard, Zoo Mode, Profile and Settings.

**Stage 6 — Verification and polish.** Test the main interactions, check permission and error states, fix TypeScript or build errors introduced by the work, and refine visual consistency.

Do not stop after Stage 1 or merely produce an implementation plan. Continue making changes in the current task. If a tool or platform limitation prevents a feature from working, implement an honest fallback and document the limitation instead of pretending it works.

## 18. Definition of Done

The work is complete when:

- All required screens are present and navigable.
- The interface is consistently designed for phones.
- In-app camera capture and gallery selection are implemented.
- Audio recording and playback are implemented where the device and compatible Expo modules support them.
- The four quest modes use a shared, expandable configuration system.
- Scoring uses configurable rarity-based points and weekly point targets, not fixed species-count requirements.
- Duplicate observations do not generate duplicate unique-species rewards.
- Collection data persists independently of weekly progress.
- All major buttons, filters, controls and forms have meaningful behaviour.
- Mock content and unavailable services are clearly identified.
- Existing backend code and unrelated project files remain untouched.
- Available type checks, lint checks and relevant tests have been run, and errors introduced by the implementation have been fixed.

At the end, provide:
1. A concise summary of what was implemented.
2. The important files created or modified.
3. The exact commands needed to run the mobile app.
4. The checks performed and any unresolved errors.
5. A clear list of which features still use mock data or have device/platform limitations.

**Final priority:** protect existing code, build a coherent mobile app, make the core flows genuinely functional, preserve data consistency, and then refine the visual design. Do not sacrifice a working implementation merely to add more decorative screens.