# group `CommonElements_group` {#group__CommonElements__group}

Elements that are not tied to any particular module.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`enum `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)            | Possible error codes for [Nuitrack](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Nuitrack) internal functions.
`enum `[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)            | Describes an issue identifier.
`class `[`tdv::nuitrack::Nuitrack`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Nuitrack) | Central class for common Nuitrack operations.
`class `[`tdv::nuitrack::Exception`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) | Common Nuitrack exception class.
`class `[`tdv::nuitrack::TerminateException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1TerminateException) | 
`class `[`tdv::nuitrack::BadConfigValueException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1BadConfigValueException) | 
`class `[`tdv::nuitrack::ConfigNotFoundException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ConfigNotFoundException) | 
`class `[`tdv::nuitrack::ModuleNotFoundException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ModuleNotFoundException) | 
`class `[`tdv::nuitrack::LicenseNotAcquiredException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException) | 
`class `[`tdv::nuitrack::ModuleNotInitializedException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ModuleNotInitializedException) | 
`class `[`tdv::nuitrack::ModuleNotStartedException`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ModuleNotStartedException) | 
`class `[`tdv::nuitrack::Frame`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Frame) | Represents a generalized frame.
`class `[`tdv::nuitrack::FrameBorderIssue`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1FrameBorderIssue) | Represents the frame bodrer issue.
`class `[`tdv::nuitrack::Issue`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Issue) | Stores general information about a issue.
`class `[`tdv::nuitrack::IssuesData`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1IssuesData) | Stores results of issue detection.
`class `[`tdv::nuitrack::BaseObjectData`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1BaseObjectData) | Generalized class for data with a timestamp.
`class `[`tdv::nuitrack::ObjectData`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ObjectData) | Generalized template for data with a timestamp.
`class `[`tdv::nuitrack::OcclusionIssue`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1OcclusionIssue) | Represents the occlusion issue.
`class `[`tdv::nuitrack::SensorIssue`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1SensorIssue) | Represents the sensor issue.
`struct `[`tdv::nuitrack::Orientation`](api-CommonElements_group.md#structtdv_1_1nuitrack_1_1Orientation) | Stores the spatial orientation as a 3x3 rotation matrix.

## Members

#### `enum `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d) {#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d}

 Values                         | Descriptions                                
--------------------------------|---------------------------------------------
OK            | No exception.
EXCEPTION            | [tdv::nuitrack::Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception)
TERMINATE_EXCEPTION            | [tdv::nuitrack::TerminateException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1TerminateException)
BAD_CONFIG_VALUE_EXCEPTION            | [tdv::nuitrack::BadConfigValueException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1BadConfigValueException)
CONFIG_NOT_FOUND_EXCEPTION            | [tdv::nuitrack::ConfigNotFoundException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ConfigNotFoundException)
MODUDLE_NOT_FOUND_EXCEPTION            | [tdv::nuitrack::ModuleNotFoundException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ModuleNotFoundException)
LICENSE_NOT_ACQUIRED_EXCEPTION            | [tdv::nuitrack::LicenseNotAcquiredException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException)
MODULE_NOT_INITIALIZED_EXCEPTION            | [tdv::nuitrack::ModuleNotInitializedException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ModuleNotInitializedException)
MODULE_NOT_STARTED_EXCEPTION            | [tdv::nuitrack::ModuleNotStartedException](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ModuleNotStartedException)

Possible error codes for [Nuitrack](#classtdv_1_1nuitrack_1_1Nuitrack) internal functions.

Each member of the enumeration corresponds to the [Nuitrack](#classtdv_1_1nuitrack_1_1Nuitrack) exception, except for the OK member, to which there is no exception.

#### `enum `[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc) {#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc}

 Values                         | Descriptions                                
--------------------------------|---------------------------------------------
NONE_ISSUE            | 
FRAME_BORDER_ISSUE            | 
OCCLUSION_ISSUE            | 
SENSOR_ISSUE            | 

Describes an issue identifier.

# class `tdv::nuitrack::Nuitrack` {#classtdv_1_1nuitrack_1_1Nuitrack}

Central class for common Nuitrack operations.

This class is an access point for common Nuitrack operations. This class does not require instance creation, all member functions are static.

Some issues that may occur when using sensor (user occluded by objects / stays close to sensor FOV borders) can be handled with the [Nuitrack::OnIssueUpdate](#classtdv_1_1nuitrack_1_1Nuitrack_1a658b417e618db5a41e2c362722fd1d49) callback. Use [Nuitrack::connectOnIssuesUpdate](#classtdv_1_1nuitrack_1_1Nuitrack_1a275eb8e5782d58629961c4a7e77c9443) to receive information about issues happening.

Callbacks for updating data from Nuitrack modules and the [Nuitrack](#classtdv_1_1nuitrack_1_1Nuitrack) central class are not automatically called. You should request new data with [Nuitrack::update](#classtdv_1_1nuitrack_1_1Nuitrack_1ab3a7cab30462af06aeeeff723a4bf54a) or [Nuitrack::waitUpdate](#classtdv_1_1nuitrack_1_1Nuitrack_1afef775c16e4ea012213daa5422227449) methods.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`typedef `[`OnIssueUpdate`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Nuitrack_1a658b417e618db5a41e2c362722fd1d49) | Callback type of the issue update request.

## Members

#### `typedef `[`OnIssueUpdate`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Nuitrack_1a658b417e618db5a41e2c362722fd1d49) {#classtdv_1_1nuitrack_1_1Nuitrack_1a658b417e618db5a41e2c362722fd1d49}

Callback type of the issue update request.

**See also**: [tdv::nuitrack::IssuesData](#classtdv_1_1nuitrack_1_1IssuesData)

[tdv::nuitrack::Issue](#classtdv_1_1nuitrack_1_1Issue)

[connectOnIssuesUpdate](#classtdv_1_1nuitrack_1_1Nuitrack_1a275eb8e5782d58629961c4a7e77c9443)

# class `tdv::nuitrack::Exception` {#classtdv_1_1nuitrack_1_1Exception}

```
class tdv::nuitrack::Exception
  : public std::runtime_error
```  

Common Nuitrack exception class.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`Exception`](#classtdv_1_1nuitrack_1_1Exception_1ad61bf2265abe466420b5c87eeb38fc69)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1Exception_1a7d88126b64fd4087cd37f1b23d2742e1)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`Exception`](#classtdv_1_1nuitrack_1_1Exception_1ad61bf2265abe466420b5c87eeb38fc69)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1Exception_1ad61bf2265abe466420b5c87eeb38fc69}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1Exception_1a7d88126b64fd4087cd37f1b23d2742e1)`() const` {#classtdv_1_1nuitrack_1_1Exception_1a7d88126b64fd4087cd37f1b23d2742e1}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::TerminateException` {#classtdv_1_1nuitrack_1_1TerminateException}

```
class tdv::nuitrack::TerminateException
  : public tdv::nuitrack::Exception
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`TerminateException`](#classtdv_1_1nuitrack_1_1TerminateException_1a17c75932ebf39008985f2b7c58ca89de)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1TerminateException_1a72c076165cf6e5d9729e34684028d010)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`TerminateException`](#classtdv_1_1nuitrack_1_1TerminateException_1a17c75932ebf39008985f2b7c58ca89de)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1TerminateException_1a17c75932ebf39008985f2b7c58ca89de}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1TerminateException_1a72c076165cf6e5d9729e34684028d010)`() const` {#classtdv_1_1nuitrack_1_1TerminateException_1a72c076165cf6e5d9729e34684028d010}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::BadConfigValueException` {#classtdv_1_1nuitrack_1_1BadConfigValueException}

```
class tdv::nuitrack::BadConfigValueException
  : public tdv::nuitrack::Exception
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`BadConfigValueException`](#classtdv_1_1nuitrack_1_1BadConfigValueException_1a70758616278e189934b4f5b1dddea5e8)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1BadConfigValueException_1aeff9727544143eefd2ef9421b6a035b5)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`BadConfigValueException`](#classtdv_1_1nuitrack_1_1BadConfigValueException_1a70758616278e189934b4f5b1dddea5e8)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1BadConfigValueException_1a70758616278e189934b4f5b1dddea5e8}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1BadConfigValueException_1aeff9727544143eefd2ef9421b6a035b5)`() const` {#classtdv_1_1nuitrack_1_1BadConfigValueException_1aeff9727544143eefd2ef9421b6a035b5}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::ConfigNotFoundException` {#classtdv_1_1nuitrack_1_1ConfigNotFoundException}

```
class tdv::nuitrack::ConfigNotFoundException
  : public tdv::nuitrack::TerminateException
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`ConfigNotFoundException`](#classtdv_1_1nuitrack_1_1ConfigNotFoundException_1af9d344dfa0b81fa35f814be1c8ff21f7)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ConfigNotFoundException_1a19641c02842032b8ecd79d1aec74aa72)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`ConfigNotFoundException`](#classtdv_1_1nuitrack_1_1ConfigNotFoundException_1af9d344dfa0b81fa35f814be1c8ff21f7)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1ConfigNotFoundException_1af9d344dfa0b81fa35f814be1c8ff21f7}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ConfigNotFoundException_1a19641c02842032b8ecd79d1aec74aa72)`() const` {#classtdv_1_1nuitrack_1_1ConfigNotFoundException_1a19641c02842032b8ecd79d1aec74aa72}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::ModuleNotFoundException` {#classtdv_1_1nuitrack_1_1ModuleNotFoundException}

```
class tdv::nuitrack::ModuleNotFoundException
  : public tdv::nuitrack::TerminateException
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`ModuleNotFoundException`](#classtdv_1_1nuitrack_1_1ModuleNotFoundException_1a5bc57441feb103dbe12772848b9f590c)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ModuleNotFoundException_1a9753a0bf6d194b6728dd6eceed01ee58)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`ModuleNotFoundException`](#classtdv_1_1nuitrack_1_1ModuleNotFoundException_1a5bc57441feb103dbe12772848b9f590c)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1ModuleNotFoundException_1a5bc57441feb103dbe12772848b9f590c}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ModuleNotFoundException_1a9753a0bf6d194b6728dd6eceed01ee58)`() const` {#classtdv_1_1nuitrack_1_1ModuleNotFoundException_1a9753a0bf6d194b6728dd6eceed01ee58}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::LicenseNotAcquiredException` {#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException}

```
class tdv::nuitrack::LicenseNotAcquiredException
  : public tdv::nuitrack::TerminateException
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`LicenseNotAcquiredException`](#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException_1ac6fa8058784cda27bc4c38c04bd11977)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException_1ad2f57203bda90ba28a2d5c89b37e925c)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`LicenseNotAcquiredException`](#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException_1ac6fa8058784cda27bc4c38c04bd11977)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException_1ac6fa8058784cda27bc4c38c04bd11977}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException_1ad2f57203bda90ba28a2d5c89b37e925c)`() const` {#classtdv_1_1nuitrack_1_1LicenseNotAcquiredException_1ad2f57203bda90ba28a2d5c89b37e925c}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::ModuleNotInitializedException` {#classtdv_1_1nuitrack_1_1ModuleNotInitializedException}

```
class tdv::nuitrack::ModuleNotInitializedException
  : public tdv::nuitrack::TerminateException
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`ModuleNotInitializedException`](#classtdv_1_1nuitrack_1_1ModuleNotInitializedException_1aef4087724e20eec833758da57980e652)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ModuleNotInitializedException_1a0b59930b6e1658f430797a31623ddb6d)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`ModuleNotInitializedException`](#classtdv_1_1nuitrack_1_1ModuleNotInitializedException_1aef4087724e20eec833758da57980e652)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1ModuleNotInitializedException_1aef4087724e20eec833758da57980e652}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ModuleNotInitializedException_1a0b59930b6e1658f430797a31623ddb6d)`() const` {#classtdv_1_1nuitrack_1_1ModuleNotInitializedException_1a0b59930b6e1658f430797a31623ddb6d}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::ModuleNotStartedException` {#classtdv_1_1nuitrack_1_1ModuleNotStartedException}

```
class tdv::nuitrack::ModuleNotStartedException
  : public tdv::nuitrack::TerminateException
```  

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`ModuleNotStartedException`](#classtdv_1_1nuitrack_1_1ModuleNotStartedException_1a7a146a2a27e262da65205f4fab6105ba)`(const std::string & msg)` | 
`public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ModuleNotStartedException_1aea7ef64a02431f9433401276e75422ed)`() const` | [Exception](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Exception) type as enum value.

## Members

#### `public inline  `[`ModuleNotStartedException`](#classtdv_1_1nuitrack_1_1ModuleNotStartedException_1a7a146a2a27e262da65205f4fab6105ba)`(const std::string & msg)` {#classtdv_1_1nuitrack_1_1ModuleNotStartedException_1a7a146a2a27e262da65205f4fab6105ba}

#### `public inline virtual `[`ExceptionType`](#group__CommonElements__group_1ga9384bcd8d6360bf085e14e381d843c7d)` `[`type`](#classtdv_1_1nuitrack_1_1ModuleNotStartedException_1aea7ef64a02431f9433401276e75422ed)`() const` {#classtdv_1_1nuitrack_1_1ModuleNotStartedException_1aea7ef64a02431f9433401276e75422ed}

[Exception](#classtdv_1_1nuitrack_1_1Exception) type as enum value.

**See also**: ExceptionType

# class `tdv::nuitrack::Frame` {#classtdv_1_1nuitrack_1_1Frame}

```
class tdv::nuitrack::Frame
  : public tdv::nuitrack::ObjectData< TImpl >
```  

Represents a generalized frame.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline virtual  `[`~Frame`](#classtdv_1_1nuitrack_1_1Frame_1a59b694ebfb03573b0298b00cfe9288a1)`()` | 
`public int `[`getRows`](#classtdv_1_1nuitrack_1_1Frame_1add39363257cbc787fde1c5120e14fd20)`() const` | Returns the number of rows in the frame.
`public int `[`getCols`](#classtdv_1_1nuitrack_1_1Frame_1aae9c0e044ad8e06ccef0e23bbb1c4d25)`() const` | Returns the number of columns in the frame.
`public uint64_t `[`getID`](#classtdv_1_1nuitrack_1_1Frame_1af331852ec5f55c4566dfde4b88befed2)`() const` | Returns the frame ID.
`public const DataType * `[`getData`](#classtdv_1_1nuitrack_1_1Frame_1a95e57da8c257b57cb7e0d73b3d1dc31b)`() const` | Returns the frame data.
`typedef `[`DataType`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Frame_1a90d0c03ae3436025306eee2cd060ac67) | 

## Members

#### `public inline virtual  `[`~Frame`](#classtdv_1_1nuitrack_1_1Frame_1a59b694ebfb03573b0298b00cfe9288a1)`()` {#classtdv_1_1nuitrack_1_1Frame_1a59b694ebfb03573b0298b00cfe9288a1}

#### `public int `[`getRows`](#classtdv_1_1nuitrack_1_1Frame_1add39363257cbc787fde1c5120e14fd20)`() const` {#classtdv_1_1nuitrack_1_1Frame_1add39363257cbc787fde1c5120e14fd20}

Returns the number of rows in the frame.

#### `public int `[`getCols`](#classtdv_1_1nuitrack_1_1Frame_1aae9c0e044ad8e06ccef0e23bbb1c4d25)`() const` {#classtdv_1_1nuitrack_1_1Frame_1aae9c0e044ad8e06ccef0e23bbb1c4d25}

Returns the number of columns in the frame.

#### `public uint64_t `[`getID`](#classtdv_1_1nuitrack_1_1Frame_1af331852ec5f55c4566dfde4b88befed2)`() const` {#classtdv_1_1nuitrack_1_1Frame_1af331852ec5f55c4566dfde4b88befed2}

Returns the frame ID.

#### `public const DataType * `[`getData`](#classtdv_1_1nuitrack_1_1Frame_1a95e57da8c257b57cb7e0d73b3d1dc31b)`() const` {#classtdv_1_1nuitrack_1_1Frame_1a95e57da8c257b57cb7e0d73b3d1dc31b}

Returns the frame data.

#### `typedef `[`DataType`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Frame_1a90d0c03ae3436025306eee2cd060ac67) {#classtdv_1_1nuitrack_1_1Frame_1a90d0c03ae3436025306eee2cd060ac67}

# class `tdv::nuitrack::FrameBorderIssue` {#classtdv_1_1nuitrack_1_1FrameBorderIssue}

```
class tdv::nuitrack::FrameBorderIssue
  : public tdv::nuitrack::Issue
```  

Represents the frame bodrer issue.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline bool `[`isTop`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1a49f1c23067bbfd734db49aaac21bcee5)`() const` | 
`public inline bool `[`isLeft`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1aa19a5ab129986c6ea2e95a723c84e0e7)`() const` | 
`public inline bool `[`isRight`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1afe7a25b97dd7b0548f69b85675e6fff6)`() const` | 
`public inline void `[`setTop`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1accd23201c5311594f6f54744a50be2d9)`(bool isTop)` | 
`public inline void `[`setLeft`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1ae9c7375d15a897708667a6b5770cb2a6)`(bool isLeft)` | 
`public inline void `[`setRight`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1ad517e601d729c0626bd46b0f5fc6c14a)`(bool isRight)` | 
`public inline  `[`FrameBorderIssue`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1abfdcd39195b3976f29c52e9cbf98fb29)`()` | Constructs a default frame border issue object.
`public inline  `[`FrameBorderIssue`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1acfdc4fb57ea0a3b8f6e26f73146df936)`(bool left,bool right,bool top)` | Constructs a frame border issue object from its properties.
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1FrameBorderIssue_1a63c882c16fa2c4f7eff66bca1e3ffd06) | Smart pointer to access the [FrameBorderIssue](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1FrameBorderIssue) instance.

## Members

#### `public inline bool `[`isTop`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1a49f1c23067bbfd734db49aaac21bcee5)`() const` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1a49f1c23067bbfd734db49aaac21bcee5}

#### `public inline bool `[`isLeft`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1aa19a5ab129986c6ea2e95a723c84e0e7)`() const` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1aa19a5ab129986c6ea2e95a723c84e0e7}

#### `public inline bool `[`isRight`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1afe7a25b97dd7b0548f69b85675e6fff6)`() const` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1afe7a25b97dd7b0548f69b85675e6fff6}

#### `public inline void `[`setTop`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1accd23201c5311594f6f54744a50be2d9)`(bool isTop)` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1accd23201c5311594f6f54744a50be2d9}

#### `public inline void `[`setLeft`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1ae9c7375d15a897708667a6b5770cb2a6)`(bool isLeft)` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1ae9c7375d15a897708667a6b5770cb2a6}

#### `public inline void `[`setRight`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1ad517e601d729c0626bd46b0f5fc6c14a)`(bool isRight)` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1ad517e601d729c0626bd46b0f5fc6c14a}

#### `public inline  `[`FrameBorderIssue`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1abfdcd39195b3976f29c52e9cbf98fb29)`()` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1abfdcd39195b3976f29c52e9cbf98fb29}

Constructs a default frame border issue object.

#### `public inline  `[`FrameBorderIssue`](#classtdv_1_1nuitrack_1_1FrameBorderIssue_1acfdc4fb57ea0a3b8f6e26f73146df936)`(bool left,bool right,bool top)` {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1acfdc4fb57ea0a3b8f6e26f73146df936}

Constructs a frame border issue object from its properties.

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1FrameBorderIssue_1a63c882c16fa2c4f7eff66bca1e3ffd06) {#classtdv_1_1nuitrack_1_1FrameBorderIssue_1a63c882c16fa2c4f7eff66bca1e3ffd06}

Smart pointer to access the [FrameBorderIssue](#classtdv_1_1nuitrack_1_1FrameBorderIssue) instance.

# class `tdv::nuitrack::Issue` {#classtdv_1_1nuitrack_1_1Issue}

Stores general information about a issue.

Parent class of all issue classes.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline virtual std::string `[`getName`](#classtdv_1_1nuitrack_1_1Issue_1aa3100e956ecee0f8b68b52646a82fa3d)`() const` | Returns the issue name.
`public inline `[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` `[`getId`](#classtdv_1_1nuitrack_1_1Issue_1a04fab1f4bf4170f1e108cc89a137ca05)`()` | Returns the issue identifier.
`public inline  `[`Issue`](#classtdv_1_1nuitrack_1_1Issue_1ac13265e4459b8a9fea112e5b1cdcd5a0)`()` | Constructs a default issue.
`public inline  `[`Issue`](#classtdv_1_1nuitrack_1_1Issue_1a2b5dde918f14f4474022502cee358236)`(`[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` id,const std::string & name)` | Constructs an issue object from its identifier and name.
`public inline virtual  `[`~Issue`](#classtdv_1_1nuitrack_1_1Issue_1a7c26e23a34d726310b9b0ce251b381a4)`()` | 
`public inline void `[`deleteString`](#classtdv_1_1nuitrack_1_1Issue_1a8464555cbe39719e8eab3b291c04d453)`()` | Release the memory occupied by the string representation of the name.
`public inline  `[`Issue`](#classtdv_1_1nuitrack_1_1Issue_1af00043510c60e5d6647c6a9be2dfb8de)`(const `[`Issue`](#classtdv_1_1nuitrack_1_1Issue)` & issue)` | Copy constructor.
`public inline void `[`operator=`](#classtdv_1_1nuitrack_1_1Issue_1a4cc75e46eba52a173580410da0d845a0)`(const `[`Issue`](#classtdv_1_1nuitrack_1_1Issue)` & issue)` | Overloaded copy assignment operator.
`protected `[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` `[`_id`](#classtdv_1_1nuitrack_1_1Issue_1a80a95a8aa3203cf92dba25d3a110f1c9) | For internal use only.
`protected char * `[`_name`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Issue_1ad7c9a063b4bd6337dbf4a7e9d5755f48) | For internal use only.
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Issue_1accacad194583de935a2b3f56c8a3b482) | Smart pointer to access the [Issue](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Issue) instance.

## Members

#### `public inline virtual std::string `[`getName`](#classtdv_1_1nuitrack_1_1Issue_1aa3100e956ecee0f8b68b52646a82fa3d)`() const` {#classtdv_1_1nuitrack_1_1Issue_1aa3100e956ecee0f8b68b52646a82fa3d}

Returns the issue name.

#### `public inline `[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` `[`getId`](#classtdv_1_1nuitrack_1_1Issue_1a04fab1f4bf4170f1e108cc89a137ca05)`()` {#classtdv_1_1nuitrack_1_1Issue_1a04fab1f4bf4170f1e108cc89a137ca05}

Returns the issue identifier.

#### `public inline  `[`Issue`](#classtdv_1_1nuitrack_1_1Issue_1ac13265e4459b8a9fea112e5b1cdcd5a0)`()` {#classtdv_1_1nuitrack_1_1Issue_1ac13265e4459b8a9fea112e5b1cdcd5a0}

Constructs a default issue.

#### `public inline  `[`Issue`](#classtdv_1_1nuitrack_1_1Issue_1a2b5dde918f14f4474022502cee358236)`(`[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` id,const std::string & name)` {#classtdv_1_1nuitrack_1_1Issue_1a2b5dde918f14f4474022502cee358236}

Constructs an issue object from its identifier and name.

#### `public inline virtual  `[`~Issue`](#classtdv_1_1nuitrack_1_1Issue_1a7c26e23a34d726310b9b0ce251b381a4)`()` {#classtdv_1_1nuitrack_1_1Issue_1a7c26e23a34d726310b9b0ce251b381a4}

#### `public inline void `[`deleteString`](#classtdv_1_1nuitrack_1_1Issue_1a8464555cbe39719e8eab3b291c04d453)`()` {#classtdv_1_1nuitrack_1_1Issue_1a8464555cbe39719e8eab3b291c04d453}

Release the memory occupied by the string representation of the name.

#### `public inline  `[`Issue`](#classtdv_1_1nuitrack_1_1Issue_1af00043510c60e5d6647c6a9be2dfb8de)`(const `[`Issue`](#classtdv_1_1nuitrack_1_1Issue)` & issue)` {#classtdv_1_1nuitrack_1_1Issue_1af00043510c60e5d6647c6a9be2dfb8de}

Copy constructor.

#### `public inline void `[`operator=`](#classtdv_1_1nuitrack_1_1Issue_1a4cc75e46eba52a173580410da0d845a0)`(const `[`Issue`](#classtdv_1_1nuitrack_1_1Issue)` & issue)` {#classtdv_1_1nuitrack_1_1Issue_1a4cc75e46eba52a173580410da0d845a0}

Overloaded copy assignment operator.

#### `protected `[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` `[`_id`](#classtdv_1_1nuitrack_1_1Issue_1a80a95a8aa3203cf92dba25d3a110f1c9) {#classtdv_1_1nuitrack_1_1Issue_1a80a95a8aa3203cf92dba25d3a110f1c9}

For internal use only.

#### `protected char * `[`_name`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Issue_1ad7c9a063b4bd6337dbf4a7e9d5755f48) {#classtdv_1_1nuitrack_1_1Issue_1ad7c9a063b4bd6337dbf4a7e9d5755f48}

For internal use only.

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1Issue_1accacad194583de935a2b3f56c8a3b482) {#classtdv_1_1nuitrack_1_1Issue_1accacad194583de935a2b3f56c8a3b482}

Smart pointer to access the [Issue](#classtdv_1_1nuitrack_1_1Issue) instance.

# class `tdv::nuitrack::IssuesData` {#classtdv_1_1nuitrack_1_1IssuesData}

Stores results of issue detection.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`IssuesData`](#classtdv_1_1nuitrack_1_1IssuesData_1acec3bb6e740f7243f95c3e48a6c5f218)`(IssueTrackerData * pimpl)` | For internal use only.
`public inline virtual  `[`~IssuesData`](#classtdv_1_1nuitrack_1_1IssuesData_1ab359a5c7ac8e5864d17e596955a39f61)`()` | 
`public template<>`  <br/>`inline std::shared_ptr< T > `[`getIssue`](#classtdv_1_1nuitrack_1_1IssuesData_1a066650378f975c687009b553f902b62f)`() const` | Returns information about sensor related issue detected.
`public template<>`  <br/>`inline std::shared_ptr< T > `[`getUserIssue`](#classtdv_1_1nuitrack_1_1IssuesData_1a45791f9a3d8eef11513256a6aec071c6)`(int userId) const` | Returns information about user related issue detected.
`protected IssueTrackerData * `[`_pimpl`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1IssuesData_1a28d7c84047cdf11be90246dcd146fa65) | For internal use only.
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1IssuesData_1a44ffff6fc3dda30426e32b444b3dde96) | Smart pointer to access the [IssuesData](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1IssuesData) instance.

## Members

#### `public inline  `[`IssuesData`](#classtdv_1_1nuitrack_1_1IssuesData_1acec3bb6e740f7243f95c3e48a6c5f218)`(IssueTrackerData * pimpl)` {#classtdv_1_1nuitrack_1_1IssuesData_1acec3bb6e740f7243f95c3e48a6c5f218}

For internal use only.

#### `public inline virtual  `[`~IssuesData`](#classtdv_1_1nuitrack_1_1IssuesData_1ab359a5c7ac8e5864d17e596955a39f61)`()` {#classtdv_1_1nuitrack_1_1IssuesData_1ab359a5c7ac8e5864d17e596955a39f61}

#### `public template<>`  <br/>`inline std::shared_ptr< T > `[`getIssue`](#classtdv_1_1nuitrack_1_1IssuesData_1a066650378f975c687009b553f902b62f)`() const` {#classtdv_1_1nuitrack_1_1IssuesData_1a066650378f975c687009b553f902b62f}

Returns information about sensor related issue detected.

#### Parameters
* `T` A data type that stores information about the requested issue.

#### `public template<>`  <br/>`inline std::shared_ptr< T > `[`getUserIssue`](#classtdv_1_1nuitrack_1_1IssuesData_1a45791f9a3d8eef11513256a6aec071c6)`(int userId) const` {#classtdv_1_1nuitrack_1_1IssuesData_1a45791f9a3d8eef11513256a6aec071c6}

Returns information about user related issue detected.

#### Parameters
* `T` A data type that stores information about the requested issue. 

#### Parameters
* `userId` ID of the user of interest.

#### `protected IssueTrackerData * `[`_pimpl`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1IssuesData_1a28d7c84047cdf11be90246dcd146fa65) {#classtdv_1_1nuitrack_1_1IssuesData_1a28d7c84047cdf11be90246dcd146fa65}

For internal use only.

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1IssuesData_1a44ffff6fc3dda30426e32b444b3dde96) {#classtdv_1_1nuitrack_1_1IssuesData_1a44ffff6fc3dda30426e32b444b3dde96}

Smart pointer to access the [IssuesData](#classtdv_1_1nuitrack_1_1IssuesData) instance.

# class `tdv::nuitrack::BaseObjectData` {#classtdv_1_1nuitrack_1_1BaseObjectData}

Generalized class for data with a timestamp.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public uint64_t `[`getTimestamp`](#classtdv_1_1nuitrack_1_1BaseObjectData_1a481fb14aeec5ba09db29a7c43784fd1c)`() const` | Returns the data timestamp in microseconds.
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1BaseObjectData_1ac5f508ca7acb29f58604471e1dc4fb17) | 

## Members

#### `public uint64_t `[`getTimestamp`](#classtdv_1_1nuitrack_1_1BaseObjectData_1a481fb14aeec5ba09db29a7c43784fd1c)`() const` {#classtdv_1_1nuitrack_1_1BaseObjectData_1a481fb14aeec5ba09db29a7c43784fd1c}

Returns the data timestamp in microseconds.

The timestamp characterizes the time point to which the data corresponds. The exact meaning of this value depends on the depth provider.

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1BaseObjectData_1ac5f508ca7acb29f58604471e1dc4fb17) {#classtdv_1_1nuitrack_1_1BaseObjectData_1ac5f508ca7acb29f58604471e1dc4fb17}

# class `tdv::nuitrack::ObjectData` {#classtdv_1_1nuitrack_1_1ObjectData}

```
class tdv::nuitrack::ObjectData
  : public tdv::nuitrack::BaseObjectData
```  

Generalized template for data with a timestamp.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline virtual  `[`~ObjectData`](#classtdv_1_1nuitrack_1_1ObjectData_1a9d2a0e19d3bc3cf44094ad990cf75298)`()` | 
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ObjectData_1af759b2c1e499ed0c0ef854e78aaba0ee) | 

## Members

#### `public inline virtual  `[`~ObjectData`](#classtdv_1_1nuitrack_1_1ObjectData_1a9d2a0e19d3bc3cf44094ad990cf75298)`()` {#classtdv_1_1nuitrack_1_1ObjectData_1a9d2a0e19d3bc3cf44094ad990cf75298}

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1ObjectData_1af759b2c1e499ed0c0ef854e78aaba0ee) {#classtdv_1_1nuitrack_1_1ObjectData_1af759b2c1e499ed0c0ef854e78aaba0ee}

# class `tdv::nuitrack::OcclusionIssue` {#classtdv_1_1nuitrack_1_1OcclusionIssue}

```
class tdv::nuitrack::OcclusionIssue
  : public tdv::nuitrack::Issue
```  

Represents the occlusion issue.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`OcclusionIssue`](#classtdv_1_1nuitrack_1_1OcclusionIssue_1ac93d2a18b62f64589c659ca911cd6cd5)`()` | Constructs a default occlusion issue object.
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1OcclusionIssue_1aa6eda1f8533d8773475901275fe810d3) | Smart pointer to access the [OcclusionIssue](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1OcclusionIssue) instance.

## Members

#### `public inline  `[`OcclusionIssue`](#classtdv_1_1nuitrack_1_1OcclusionIssue_1ac93d2a18b62f64589c659ca911cd6cd5)`()` {#classtdv_1_1nuitrack_1_1OcclusionIssue_1ac93d2a18b62f64589c659ca911cd6cd5}

Constructs a default occlusion issue object.

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1OcclusionIssue_1aa6eda1f8533d8773475901275fe810d3) {#classtdv_1_1nuitrack_1_1OcclusionIssue_1aa6eda1f8533d8773475901275fe810d3}

Smart pointer to access the [OcclusionIssue](#classtdv_1_1nuitrack_1_1OcclusionIssue) instance.

# class `tdv::nuitrack::SensorIssue` {#classtdv_1_1nuitrack_1_1SensorIssue}

```
class tdv::nuitrack::SensorIssue
  : public tdv::nuitrack::Issue
```  

Represents the sensor issue.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public inline  `[`SensorIssue`](#classtdv_1_1nuitrack_1_1SensorIssue_1a4a6c768ffce157fb67ae6d8725da2d8c)`(`[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` issueId,std::string issueName)` | Constructs a sensor issue object from its ID and name.
`typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1SensorIssue_1aa2f1c4c81cf15e5a2b631053e4302774) | Smart pointer to access the [SensorIssue](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1SensorIssue) instance.

## Members

#### `public inline  `[`SensorIssue`](#classtdv_1_1nuitrack_1_1SensorIssue_1a4a6c768ffce157fb67ae6d8725da2d8c)`(`[`IssueId`](#group__CommonElements__group_1gaabc4e793011f5ae40d6f8c6c977f0cfc)` issueId,std::string issueName)` {#classtdv_1_1nuitrack_1_1SensorIssue_1a4a6c768ffce157fb67ae6d8725da2d8c}

Constructs a sensor issue object from its ID and name.

#### `typedef `[`Ptr`](api-CommonElements_group.md#classtdv_1_1nuitrack_1_1SensorIssue_1aa2f1c4c81cf15e5a2b631053e4302774) {#classtdv_1_1nuitrack_1_1SensorIssue_1aa2f1c4c81cf15e5a2b631053e4302774}

Smart pointer to access the [SensorIssue](#classtdv_1_1nuitrack_1_1SensorIssue) instance.

# struct `tdv::nuitrack::Orientation` {#structtdv_1_1nuitrack_1_1Orientation}

Stores the spatial orientation as a 3x3 rotation matrix.

## Summary

 Members                        | Descriptions                                
--------------------------------|---------------------------------------------
`public float `[`matrix`](api-CommonElements_group.md#structtdv_1_1nuitrack_1_1Orientation_1a2e256a2bf249531b9b12fdd9b5c79d55) | Flattened 3x3 rotation matrix.
`public inline  `[`Orientation`](#structtdv_1_1nuitrack_1_1Orientation_1a63e7e5edc328c18fedb87c33e8fff89a)`()` | Constructs an orientation with the zero matrix.

## Members

#### `public float `[`matrix`](api-CommonElements_group.md#structtdv_1_1nuitrack_1_1Orientation_1a2e256a2bf249531b9b12fdd9b5c79d55) {#structtdv_1_1nuitrack_1_1Orientation_1a2e256a2bf249531b9b12fdd9b5c79d55}

Flattened 3x3 rotation matrix.

#### `public inline  `[`Orientation`](#structtdv_1_1nuitrack_1_1Orientation_1a63e7e5edc328c18fedb87c33e8fff89a)`()` {#structtdv_1_1nuitrack_1_1Orientation_1a63e7e5edc328c18fedb87c33e8fff89a}

Constructs an orientation with the zero matrix.

