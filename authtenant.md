---
description: >-
  All users of the Riset.ai API must register using the method described in this
  document.
---

# AuthTenant

## **1. POST `/oauth/client/token`**

### **Description**

This endpoint is used to perform operations related to accounts in **AuthTenant**. For now, this functionality is limited to **Accesstoken** requests.

Will generate a new **Accesstoken** every time it gets a request, it is recommended to take notes.

### **Request**

#### **`Headers`**

* `None`

#### **`Body`**

* `x-www-form-urlencoded`

| KEY            | VALUE       |
| -------------- | ----------- |
| **client\_id** | `riset.ai`  |
| **password**   | `#password` |

### **Response**

#### **`Headers`**

| KEY              | VALUE              |
| ---------------- | ------------------ |
| **Content-Type** | `application/json` |

#### **`Body`**

```javascript
{
  "access_token": "eyJhbGciOiJSUzI1NiIsIn..."
}
```

## **2. POST `/oauth/token/revoke`**

### **Description**

This endpoint is used to revoke the previously generated **Accesstoken**. **Accesstoken** will be added to the Blacklist list.

Used if **Accesstoken** leaked to other party, and want to revoke the authorization.

### **Request**

#### **`Headers`**

| KEY             | VALUE               |
| --------------- | ------------------- |
| **Accesstoken** | `oauth Accesstoken` |

#### Body

* `x-www-form-urlencoded`

| KEY            | VALUE       |
| -------------- | ----------- |
| **client\_id** | `riset.ai`  |
| **password**   | `#password` |

### **Response**

#### **`Headers`**

| KEY              | VALUE              |
| ---------------- | ------------------ |
| **Content-Type** | `application/json` |

#### **`Body`**

```javascript
{
  "status_message": "Success Revoke Token"
}
```
