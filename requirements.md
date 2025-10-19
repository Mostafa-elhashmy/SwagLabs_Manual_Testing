# Swag Labs – Requirements (Professional Edition)

## 0. Overview & Scope
This document defines functional and non-functional requirements for core Swag Labs flows:
**Login, Inventory, Product Details, Cart, Checkout (Info → Overview → Complete), and Session/Logout**.

### 0.1 Goals
- Deliver a clear, testable specification for core shopper journeys.
- Enable traceability from Acceptance Criteria (AC) to test cases and results.
- Reduce ambiguity by defining edge cases and negative paths.

### 0.2 Glossary
- **AC**: Acceptance Criteria (testable condition of satisfaction)
- **Inventory**: product listing page
- **Overview**: order review page (before final submit)
- **Badge**: cart icon counter in header

### 0.3 Out of Scope (initial)
- Real payments, email delivery verification, localization, deep performance benchmarking.

### 0.4 Assumptions
- Public demo credentials are available (e.g., `standard_user`, `locked_out_user`, `problem_user`).
- Test environment: https://www.saucedemo.com/

---

## US-1 Login
**As a shopper**, I want to log in using a username and password so that I can access the store.

**Acceptance Criteria**
- **US-1 AC1 (Valid login):** Given a valid username/password, when I click **Login**, then I am redirected to **Inventory**.
- **US-1 AC2 (Invalid login):** Given an invalid username or password, then an **error banner** is shown and I remain on **Login**.
- **US-1 AC3 (Field behavior):** Password is **masked**; both fields allow paste; leading/trailing spaces are ignored.
- **US-1 AC4 (Locked user):** Given a **locked_out_user**, login shows the **locked** error message and access is denied.
- **US-1 AC5 (Direct access control):** After **Logout**, navigating to `/inventory` redirects back to **Login**.

## US-2 Inventory Browsing & Sorting
**As a shopper**, I want to view and sort products so I can find items easily.

**Acceptance Criteria**
- **US-2 AC1 (Display):** Each product shows **name, price, image, and Add/Remove button**.
- **US-2 AC2 (Sort options):** Sort by **Name (A→Z, Z→A)** and **Price (low→high, high→low)** is available and applied immediately.
- **US-2 AC3 (Sort correctness):** When sorting by price low→high, prices render in **non-decreasing** order; by name A→Z, names render in **ascending** lexicographic order.
- **US-2 AC4 (Navigation):** Clicking a product card opens **Product Details**; clicking **Back** returns to the same list and **preserves** the last sort selection.

## US-3 Product Details
**As a shopper**, I want to see a product’s details so I can decide to buy it.

**Acceptance Criteria**
- **US-3 AC1 (Content):** Details page shows **name, description, price, image**, and **Add/Remove** button.
- **US-3 AC2 (Consistency):** The **name and price** match those shown on **Inventory**.
- **US-3 AC3 (Back):** Clicking **Back** returns to Inventory and **preserves** previous sort.

## US-4 Cart – Add/Remove & Badge
**As a shopper**, I want to add/remove items and see my cart count so I can manage my basket.

**Acceptance Criteria**
- **US-4 AC1 (Toggle):** Clicking **Add to cart** changes the same control to **Remove** (and vice versa) wherever it appears (Inventory or Details).
- **US-4 AC2 (Badge):** The cart **badge** equals the **count of distinct items** in the cart; when the cart is empty, the badge is **cleared**.
- **US-4 AC3 (Persistence):** Cart state remains **consistent** when navigating between Inventory, Details, and Cart pages.
- **US-4 AC4 (Remove in Cart):** Removing an item from **Cart** updates the **badge** and removes the line item immediately.

## US-5 Checkout – Your Information
**As a shopper**, I want to enter my shipping info so that I can proceed to order review.

**Acceptance Criteria**
- **US-5 AC1 (Required):** **First Name, Last Name, Postal Code** are required; leaving any blank shows an **inline error** and blocks **Continue**.
- **US-5 AC2 (Field handling):** Inputs accept standard alphanumeric characters; leading/trailing spaces are ignored in validation.
- **US-5 AC3 (Continue):** With valid inputs, clicking **Continue** navigates to **Overview**.

## US-6 Checkout – Overview & Totals
**As a shopper**, I want to verify items and totals before I place my order.

**Acceptance Criteria**
- **US-6 AC1 (Line items):** Overview lists **each item name & price** and the **quantity**.
- **US-6 AC2 (Subtotal):** **Item total (subtotal)** equals the **sum of item prices** currently in the cart.
- **US-6 AC3 (Tax & total):** **Total** = **Subtotal + Tax**; math uses **two-decimal rounding**.
- **US-6 AC4 (Navigation):** **Cancel** returns to the cart; **Finish** proceeds to **Complete**.

## US-7 Order Completion
**As a shopper**, I want a confirmation of my order so I know it succeeded.

**Acceptance Criteria**
- **US-7 AC1 (Success screen):** After **Finish**, a **confirmation page** is displayed with a success message.
- **US-7 AC2 (Cart reset):** After completion, the **cart is emptied**; badge is cleared, and navigating back to Inventory shows **Add to cart** (not Remove).
- **US-7 AC3 (Idempotency/refresh):** Refreshing the **Complete** page does **not** re-submit or duplicate the order.

## US-8 Session & Logout
**As a shopper**, I want to manage my session to keep my account safe.

**Acceptance Criteria**
- **US-8 AC1 (Logout):** Selecting **Logout** from the menu returns to **Login** and clears the cart state.
- **US-8 AC2 (Direct URL access):** While logged out, visiting an authenticated page (e.g., `/cart`, `/inventory`) redirects to **Login**.

## US-9 Errors, Messages, and Validation (Cross-cutting)
**As a shopper**, I want clear feedback when something goes wrong so I can correct it.

**Acceptance Criteria**
- **US-9 AC1 (Placement):** Error messages appear **near relevant fields** or in a **visible banner** and are readable.
- **US-9 AC2 (Content):** Messages are **specific**, and do **not** expose sensitive information.
- **US-9 AC3 (Focus):** After a validation error, **keyboard focus** moves to the **first** invalid field.

---

## Non-Functional Requirements (NFR)
### NFR-1 Usability & Accessibility (quick pass)
- All interactive controls are reachable by **keyboard** with a visible focus indicator.
- Images have **text alternatives** (product name in alt or aria-label).
- Form errors are announced or clearly visible without relying on color only.

### NFR-2 Security (UI level)
- No sensitive data is displayed or stored in **URL** or client-side storage in plain text.
- Error messages do **not** reveal whether a username exists.

### NFR-3 Performance (informational)
- Inventory and Cart pages display primary content within **2 seconds** on a typical desktop connection.

---

## Definition of Ready (DoR) – for each story
- Story has **clear AC** and endpoints/URLs known.
- Test data available (e.g., `standard_user`, `locked_out_user`).
- UI states identified (loading, error messages).
- No critical dependency blocking test execution.
