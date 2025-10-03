---
title: Welcome to IddirNet API Documentation
hide:
  - toc  # Optional: Hide table of contents on this page
---

# IddirNet API

Welcome to the official documentation for the **IddirNet** RESTful API. IddirNet is a platform for managing community associations (Iddirs), enabling CRUD operations for users, groups, members, and contributions.

## Quick Start

### Prerequisites
- API Base URL: `https://iddirnet-75e4b810c131.herokuapp.com/api/v1`
- Authentication: Use JWT Bearer tokens (see [Authentication](api.md#authentication) section).
- Tools: Postman, curl, or any HTTP client.

### Example Workflow
1. **Register/Login**: Create an account or log in to get a token.
2. **Create an Iddir**: Set up a new community group.
3. **Manage Members & Contributions**: Add users and track payments.

For testing, import the [Postman Collection](https://documenter.getpostman.com/view/45526338/2sB3HqHdvd) or view the [Swagger UI](https://iddirnet-75e4b810c131.herokuapp.com/swagger/).

## Key Features
- **User Management**: Full CRUD for users with role-based access.
- **Iddir Groups**: Create and manage community associations.
- **Membership & Contributions**: Track joins and financial contributions.
- **Pagination & Filtering**: Efficient data retrieval.

## Navigation
- [API Reference →](api.md): Detailed endpoints with cURL examples.

---

<div class="admonition note">
  <p class="admonition-title">Need Help?</p>
  <p>Check the <a href="api.md#error-responses">Error Handling</a> section or contact support@iddirnet.com.</p>
</div>

![IddirNet Logo](https://via.placeholder.com/400x200?text=IddirNet) <!-- Replace with your actual logo URL -->