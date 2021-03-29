```
region name: ap-southeast-1
```

```
Create user and allocates permission for user to access manage service by aws console or aws command line.
Create group and add user into a group, and allocates policy into group. it's easy for manage permission 
Create role. using in a case lamda functionA access mongoDbB and s3C
Create policy. custom permission and action in service
```

### MFA
```
Create policy
Service: IAM
Access level: 
- List: ListVirtualMFADevices, ListUsers, ListMFADevices
- Write: DeactivateMFADevice, CreateVirtualMFADevice, EnableMFADevice, DeleteVirtualMFADevice, ResyncMFADevice

All resources
```