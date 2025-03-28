# IAM Security JSON Snippets

A collection of ready-to-use JSON configurations for implementing IAM security restrictions and following best practices across various cloud platforms and services.

## AWS IAM Policy Examples

### Least Privilege S3 Bucket Access
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::example-bucket",
        "arn:aws:s3:::example-bucket/*"
      ],
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "192.0.2.0/24"
        }
      }
    }
  ]
}
```

### Require MFA for Sensitive Actions
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:ChangePassword",
        "iam:CreateAccessKey"
      ],
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}",
      "Condition": {
        "Bool": {
          "aws:MultiFactorAuthPresent": "true"
        }
      }
    }
  ]
}
```

## Azure RBAC Definitions

### Custom Role with Limited Permissions
```json
{
  "Name": "VM Operator",
  "Description": "Can start and stop VMs but cannot modify them",
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000"
  ],
  "Actions": [
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/read"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": []
}
```

### Conditional Access Policy
```json
{
  "displayName": "Require MFA for admin accounts",
  "state": "enabled",
  "conditions": {
    "clientAppTypes": ["all"],
    "applications": {
      "includeApplications": ["All"]
    },
    "users": {
      "includeRoles": [
        "Global Administrator",
        "Security Administrator"
      ]
    }
  },
  "grantControls": {
    "operator": "AND",
    "builtInControls": ["mfa"]
  }
}
```

## GCP IAM Policy Bindings

### Project-Level Custom Role
```json
{
  "title": "Storage Viewer",
  "description": "Access to view storage objects only",
  "stage": "GA",
  "includedPermissions": [
    "storage.buckets.get",
    "storage.buckets.list",
    "storage.objects.get",
    "storage.objects.list"
  ]
}
```

### Organization Policy Constraint
```json
{
  "constraint": "constraints/iam.disableServiceAccountKeyCreation",
  "listPolicy": {
    "allValues": "DENY"
  }
}
```

## JWT Access Token Structure

### Standard JWT with Role-Based Claims
```json
{
  "header": {
    "alg": "RS256",
    "typ": "JWT"
  },
  "payload": {
    "sub": "1234567890",
    "name": "John Doe",
    "roles": ["admin", "user"],
    "permissions": ["read:users", "write:users"],
    "iss": "https://auth.example.com",
    "aud": "https://api.example.com",
    "exp": 1622505266,
    "iat": 1622501666,
    "jti": "random-unique-string"
  }
}
```

## OAuth 2.0 Configuration

### Authorization Server Configuration
```json
{
  "issuer": "https://auth.example.com",
  "authorization_endpoint": "https://auth.example.com/authorize",
  "token_endpoint": "https://auth.example.com/oauth/token",
  "jwks_uri": "https://auth.example.com/.well-known/jwks.json",
  "response_types_supported": ["code", "token", "id_token"],
  "grant_types_supported": ["authorization_code", "refresh_token"],
  "subject_types_supported": ["public"],
  "id_token_signing_alg_values_supported": ["RS256"],
  "scopes_supported": ["openid", "profile", "email"]
}
```

## SAML 2.0 Service Provider Metadata

```json
{
  "entityId": "https://app.example.com",
  "assertionConsumerService": {
    "url": "https://app.example.com/saml/acs",
    "binding": "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
  },
  "singleLogoutService": {
    "url": "https://app.example.com/saml/logout",
    "binding": "urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect"
  },
  "nameIDFormat": "urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress",
  "signingCertificate": "MIIDETCCAfkCBEHMvJ..."
}
```

## Best Practices Implementation

### Zero Trust Network Access Rules
```json
{
  "rules": [
    {
      "name": "Allow corporate devices",
      "conditions": {
        "device": {
          "managed": true,
          "compliance": "compliant"
        },
        "network": {
          "ip_categories": ["corporate"]
        },
        "user": {
          "riskScore": {"max": 20}
        }
      },
      "action": "allow"
    }
  ]
}
```

### Role-Based Access Control Matrix
```json
{
  "roles": {
    "viewer": {
      "resources": {
        "documents": ["read"],
        "reports": ["read"]
      }
    },
    "editor": {
      "resources": {
        "documents": ["read", "write"],
        "reports": ["read", "write"]
      }
    },
    "admin": {
      "resources": {
        "documents": ["read", "write", "delete"],
        "reports": ["read", "write", "delete"],
        "users": ["read", "write"]
      }
    }
  }
}
```

## Contributing

1. Fork the repository
2. Add your JSON snippets following the existing structure
3. Create a pull request with a brief explanation of the added examples
