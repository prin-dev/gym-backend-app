# Personas and Application Responsibilities

## Owner

A gym business owner who needs to manage daily operations. Multiple owners of one organization have equal permissions in V1.

Owner tasks:
- Set up the gym and branch.
- Invite another owner.
- Review and approve join requests.
- Manage members and subscriptions.
- Record and confirm payments.
- Track attendance and expiry.
- Understand current operational and financial position.

## Platform Administrator

An explicitly provisioned internal operator responsible for platform-level operational oversight. This persona is distinct from gym Owners and Members and does not receive organization permissions by default.

Platform administrator tasks (proposed V1 scope):
- View platform-level operational overview.
- Search and inspect organizations using explicitly authorized, privacy-filtered fields.
- Search and inspect global user accounts using explicitly authorized fields.
- Review sanitized system health information.
- Review read-only audit events.

Platform administration must be enforced by backend global permissions. It is not an organization role and must not be granted through Owner/Member membership.

## Member

A person who may belong to one or more gyms, but can also use personal workout tracking without joining a gym.

Member tasks:
- Create an account.
- Optionally complete a personal profile.
- Find a gym using a reference code.
- Request to join and see request status.
- View membership and subscription information.
- Check in at an eligible gym.
- Track workouts and personal progress.

## App boundary

Owner and Member apps are separate applications, not modes of a universal app. The Platform Admin Dashboard is a separate internal web application and is not part of either native app. A global account may authenticate in both applications if the person has the relevant organization relationship/role.

Owner app supports switching between organizations the user owns or co-owns. Member app supports switching between organizations the user belongs to.

## Global identity versus organization role

A user can:
- Own multiple organizations by invitation.
- Be a member of multiple organizations.
- Have different roles in different organizations.
- Use the same global identity in Owner and Member apps.

V1 roles are `OWNER` and `MEMBER`. Staff roles are deferred.
