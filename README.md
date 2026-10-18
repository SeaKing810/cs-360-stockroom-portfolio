# CS 360 Portfolio: Stockroom Inventory App

**Sean McKenna**  
CS 360: Mobile Architect & Programming — Module Eight Journal

## Project artifact

[Download the completed Project Three app code design ZIP](SeanMcKenna_Option1_InventoryApp_ProjectThree.zip).

The ZIP contains the Android Studio project, Java source code, finalized XML UI layouts developed from Project Two, automated tests, emulator screenshots, and build/run instructions. After extracting it, open the enclosed `SeanMcKenna_Option1_InventoryApp_ProjectThree` folder in Android Studio. The project's own README explains setup and operation, and `docs/TEST_RESULTS.md` documents verification results and limitations.

## App requirements, goals, and user needs

I developed Stockroom, an Android inventory app designed to help users keep track of items, update quantities, and recognize when supplies run out. The requirements included account creation and login, a database-backed inventory grid, the ability to add, view, edit, and delete records, and optional SMS notifications when stock reaches zero. Users can also search by item name or SKU and filter for zero-stock items. My goal was to make routine inventory work straightforward while preserving records between sessions and keeping each account's inventory separate on the device.

## Screens, features, and user-centered design

The app needed login and account creation, an inventory grid, an add/edit item form, and an SMS settings screen. I kept common tasks close to the information users need: each inventory card displays the item name, SKU, location, and quantity, with controls for adjusting stock or editing the record. Clear field labels, validation messages, an empty-inventory state, and deletion confirmation help users understand what to do and avoid mistakes. The grid adapts to available space and text size, and users can decline SMS access without losing inventory functionality. These designs were successful in supporting the tested workflows because navigation stayed clear and users could complete the main tasks without unnecessary steps; the UI tests also verified login, item entry, search, and tablet grid behavior.

## Coding approach and reusable strategies

I used the Project Two Java/XML interface as a baseline and connected its screens to persistent data and working application logic. I separated database operations, password hashing, screen behavior, and SMS handling into focused classes so each part had a clear responsibility. Input validation, bound database query parameters, account-specific queries, and transactions helped maintain consistent records. Database work and password hashing run on a background executor to keep the interface responsive. In future projects, I can apply the same separation of responsibilities and test individual components before checking the complete user workflow, making changes easier to understand and verify.

## Testing, results, and lessons

I used automated tests, Android Lint, build checks, and hands-on emulator testing to verify functionality. The final Project Three results recorded 20 passing automated tests: nine database tests, six SMS tests, and five screen-flow tests. They covered authentication, saved inventory, account isolation, duplicate SKUs, quantity limits, SMS permission and opt-in behavior, search, rotation, logout, and tablet layouts. Emulator checks confirmed that edited inventory survived restarting the app and that denying SMS permission did not prevent stock updates. The debug build passed, and Lint reported zero errors with 14 nonblocking warnings. Testing matters because a screen can appear correct while saving the wrong data or failing in an alternate state. It confirmed the core workflows and clarified the limits of verification: the emulator exercised the SMS send path and callback, but it did not establish delivery through a physical carrier.

## Challenge and innovation

One design challenge was connecting inventory updates to SMS alerts without repeatedly notifying users about the same zero-stock condition or allowing a messaging problem to interfere with saved inventory. I addressed this by recording an alert event only when an existing item's quantity changes from a positive number to zero. The inventory change and event are saved together in a database transaction, and SMS is attempted afterward only when permission, saved opt-in, a valid recipient, and device support are available. Editing an item that is already at zero does not create another alert. This approach keeps inventory usable when permission is denied or SMS is unavailable and gives users a visible status for the alert attempt.

## Component that best demonstrates my skills

The SQLite inventory component is where I was particularly successful. It turns the interface's add, edit, delete, and quantity controls into persistent operations while enforcing account ownership, unique SKUs within each account, and valid quantity ranges. It also coordinates inventory changes with zero-stock events without depending on successful SMS delivery. The persistence and account-isolation tests support this part of the implementation. This component demonstrates my ability to connect a user-centered interface to structured data, apply validation and access boundaries, and verify that the application behaves consistently across sessions.
