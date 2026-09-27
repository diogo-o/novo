# Admin — Functional Rules

Functional authority for Admin. Access is in `shared/ACCESS_MODEL.md`.

## 1. Purpose

Admin is one assignable system module, not an operational production role. It contains:

- **Users:** create, edit, activate/deactivate, request password reset, and manage invitations;
- **Templates:** create/edit reusable access templates and associate one effective template per user;
- **Applications:** manage module presentation availability and order only;
- **Audit:** view filtered append-only business events and export a selected year when authorized.

## 2. Rules

- The catalog never grants access by itself. Templates grant modules and permissions; the actions
  granted determine what a user can do inside a granted module.
- An Admin grant does not imply operational access, and an operational grant does not imply Admin.
- Free-text job titles are visual labels only and never grant access.
- Passwords are never displayed or supplied by Admin.
- The first active Admin cannot be removed or deactivated if that would leave no active Admin.
- Audit contains factual events, not productivity scores or rankings.
- Applications administration changes presentation availability and order only; it never changes the
  functional model or the functional scope.

## 3. Boundaries

- Admin is not an operational module.
- Internal Admin surfaces are not separately assignable modules.
- Excluded domains must not reappear in Applications, navigation, or templates.

## 4. Current implementation state

See `modules/admin/IMPLEMENTATION_STATE.md`. Evidence does not create or override functional rules.
