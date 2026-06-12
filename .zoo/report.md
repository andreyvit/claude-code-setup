At the end of the normal report, add an extra “Info for conf team” section -- to pass on “what you should know” and “next steps” information for Mia, Tam and other conf team members. Summarize what we implemented (conf-visible, staff-visible and customer-visible details only, no internal kitchen, no code refs, labels/names as seen in superadmin/admin, and limited to only what conf people should know, or what they need to communicate to the clients), and action items for configuration team to handle/communicate after deployment. Use Markdown. A single list of changes, with subitems if needed (e.g. available options should be subitem on 'a new setting' item), make sure the list of not overwhelming, combine items where appropriate. And action items as markdown checkboxes; make sure each step is a separate task. Example format (adapt to realities of the ticket). Try to be brief, simple, and do NOT overdo this. If we updated any _readme/manual/*.md, link to updated files on GitHub.

--- example ---
## Info for conf team

Changes:

1. Superadmin > Venues: added frubernator setting with options:
   - Foo (default)
   - Bar -- appropriate for this ticket's Gold Rush store
   - Boz -- use when X or Y

2. New Import: added Frubernate checkbox in Customers mode, which enables XXX.

3. Alert popup after redemption: when frubernation enabled, displays an extra paragraph with frubs status, customizable via translation system, disable by setting translation string to `<EMPTY>`.

TODOs for conf team:

- [ ] Configure Gold Rush to use Bar under Superadmin > Venues
- [ ] Customize confirmation message (via Admin > Translate) if desired
- [ ] Enable Frubernate if customer's email was over 42 words long
- [ ] Verify popups still work okay on existing stores

--- end example ---
