---
title: Errors.sol
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Library for all Story Protocol contract errors.

### Governance\_\_OnlyProtocolAdmin

```solidity
error Governance__OnlyProtocolAdmin()
```

### Governance\_\_ZeroAddress

```solidity
error Governance__ZeroAddress()
```

### Governance\_\_ProtocolPaused

```solidity
error Governance__ProtocolPaused()
```

### Governance\_\_InconsistentState

```solidity
error Governance__InconsistentState()
```

### Governance\_\_NewStateIsTheSameWithOldState

```solidity
error Governance__NewStateIsTheSameWithOldState()
```

### Governance\_\_UnsupportedInterface

```solidity
error Governance__UnsupportedInterface(string interfaceName)
```

### IPAccount\_\_InvalidSigner

```solidity
error IPAccount__InvalidSigner()
```

### IPAccount\_\_InvalidSignature

```solidity
error IPAccount__InvalidSignature()
```

### IPAccount\_\_ExpiredSignature

```solidity
error IPAccount__ExpiredSignature()
```

### IPAccount\_\_InvalidCalldata

```solidity
error IPAccount__InvalidCalldata()
```

### IPAccount\_\_InvalidAccessController

```solidity
error IPAccount__InvalidAccessController()
```

### Module\_Unauthorized

```solidity
error Module_Unauthorized()
```

The caller is not allowed to call the provided module.

### IPAccountRegistry\_InvalidIpAccountImpl

```solidity
error IPAccountRegistry_InvalidIpAccountImpl()
```

### IPAssetRegistry\_\_AlreadyRegistered

```solidity
error IPAssetRegistry__AlreadyRegistered()
```

The IP asset has already been registered.

### IPAssetRegistry\_\_IPAccountAlreadyCreated

```solidity
error IPAssetRegistry__IPAccountAlreadyCreated()
```

The IP account has already been created.

### IPAssetRegistry\_\_NotYetRegistered

```solidity
error IPAssetRegistry__NotYetRegistered()
```

The IP asset has not yet been registered.

### IPAssetRegistry\_\_RegistrantUnauthorized

```solidity
error IPAssetRegistry__RegistrantUnauthorized()
```

The IP asset registrant is not authorized.

### IPAssetRegistry\_\_ResolverInvalid

```solidity
error IPAssetRegistry__ResolverInvalid()
```

The specified IP resolver is not valid.

### IPAssetRegistry\_\_Unauthorized

```solidity
error IPAssetRegistry__Unauthorized()
```

Caller not authorized to perform the IP registry function call.

### IPAssetRegistry\_\_InvalidAccount

```solidity
error IPAssetRegistry__InvalidAccount()
```

The deployed address of account doesn't match with IP ID.

### IPAssetRegistry\_\_InvalidMetadataProvider

```solidity
error IPAssetRegistry__InvalidMetadataProvider()
```

The metadata provider is not valid.

### IPResolver\_InvalidIP

```solidity
error IPResolver_InvalidIP()
```

The targeted IP does not yet have an IP account.

### IPResolver\_Unauthorized

```solidity
error IPResolver_Unauthorized()
```

Caller not authorized to perform the IP resolver function call.

### MetadataProvider\_\_HashInvalid

```solidity
error MetadataProvider__HashInvalid()
```

Provided hash metadata is not valid.

### MetadataProvider\_\_IPAssetOwnerInvalid

```solidity
error MetadataProvider__IPAssetOwnerInvalid()
```

The caller is not the authorized IP asset owner.

### MetadataProvider\_\_NameInvalid

```solidity
error MetadataProvider__NameInvalid()
```

Provided hash metadata is not valid.

### MetadataProvider\_\_MetadataNotCompatible

```solidity
error MetadataProvider__MetadataNotCompatible()
```

The new metadata provider is not compatible with the old provider.

### MetadataProvider\_\_RegistrantInvalid

```solidity
error MetadataProvider__RegistrantInvalid()
```

Provided registrant metadata is not valid.

### MetadataProvider\_\_RegistrationDateInvalid

```solidity
error MetadataProvider__RegistrationDateInvalid()
```

Provided registration date is not valid.

### MetadataProvider\_\_Unauthorized

```solidity
error MetadataProvider__Unauthorized()
```

Caller does not access to set metadata storage for the provider.

### MetadataProvider\_\_UpgradeUnavailable

```solidity
error MetadataProvider__UpgradeUnavailable()
```

A metadata provider upgrade is not currently available.

### MetadataProvider\_\_UpgradeProviderInvalid

```solidity
error MetadataProvider__UpgradeProviderInvalid()
```

The upgrade provider is not valid.

### MetadataProvider\_\_URIInvalid

```solidity
error MetadataProvider__URIInvalid()
```

Provided metadata URI is not valid.

### LicenseRegistry\_\_CallerNotLicensingModule

```solidity
error LicenseRegistry__CallerNotLicensingModule()
```

### LicenseRegistry\_\_ZeroLicensingModule

```solidity
error LicenseRegistry__ZeroLicensingModule()
```

### LicensingModule\_\_CallerNotLicenseRegistry

```solidity
error LicensingModule__CallerNotLicenseRegistry()
```

### LicenseRegistry\_\_RevokedLicense

```solidity
error LicenseRegistry__RevokedLicense()
```

### LicenseRegistry\_\_NotTransferable

```solidity
error LicenseRegistry__NotTransferable()
```

emitted when trying to transfer a license that is not transferable (by policy)

### LicenseRegistry\_\_ZeroDisputeModule

```solidity
error LicenseRegistry__ZeroDisputeModule()
```

emitted on constructor if dispute module is not set

### LicensingModule\_\_PolicyAlreadySetForIpId

```solidity
error LicensingModule__PolicyAlreadySetForIpId()
```

### LicensingModule\_\_FrameworkNotFound

```solidity
error LicensingModule__FrameworkNotFound()
```

### LicensingModule\_\_EmptyLicenseUrl

```solidity
error LicensingModule__EmptyLicenseUrl()
```

### LicensingModule\_\_InvalidPolicyFramework

```solidity
error LicensingModule__InvalidPolicyFramework()
```

### LicensingModule\_\_ParamVerifierLengthMismatch

```solidity
error LicensingModule__ParamVerifierLengthMismatch()
```

### LicensingModule\_\_PolicyNotFound

```solidity
error LicensingModule__PolicyNotFound()
```

### LicensingModule\_\_NotLicensee

```solidity
error LicensingModule__NotLicensee()
```

### LicensingModule\_\_ParentIdEqualThanChild

```solidity
error LicensingModule__ParentIdEqualThanChild()
```

### LicensingModule\_\_LicensorDoesntHaveThisPolicy

```solidity
error LicensingModule__LicensorDoesntHaveThisPolicy()
```

### LicensingModule\_\_MintLicenseParamFailed

```solidity
error LicensingModule__MintLicenseParamFailed()
```

### LicensingModule\_\_LinkParentParamFailed

```solidity
error LicensingModule__LinkParentParamFailed()
```

### LicensingModule\_\_TransferParamFailed

```solidity
error LicensingModule__TransferParamFailed()
```

### LicensingModule\_\_InvalidLicensor

```solidity
error LicensingModule__InvalidLicensor()
```

### LicensingModule\_\_ParamVerifierAlreadySet

```solidity
error LicensingModule__ParamVerifierAlreadySet()
```

### LicensingModule\_\_CommercialTermInNonCommercialPolicy

```solidity
error LicensingModule__CommercialTermInNonCommercialPolicy()
```

### LicensingModule\_\_EmptyParamName

```solidity
error LicensingModule__EmptyParamName()
```

### LicensingModule\_\_UnregisteredFrameworkAddingPolicy

```solidity
error LicensingModule__UnregisteredFrameworkAddingPolicy()
```

### LicensingModule\_\_UnauthorizedAccess

```solidity
error LicensingModule__UnauthorizedAccess()
```

### LicensingModule\_\_LicensorNotRegistered

```solidity
error LicensingModule__LicensorNotRegistered()
```

### LicensingModule\_\_CallerNotLicensorAndPolicyNotSet

```solidity
error LicensingModule__CallerNotLicensorAndPolicyNotSet()
```

### LicensingModule\_\_DerivativesCannotAddPolicy

```solidity
error LicensingModule__DerivativesCannotAddPolicy()
```

### LicensingModule\_\_IncompatibleRoyaltyPolicyAddress

```solidity
error LicensingModule__IncompatibleRoyaltyPolicyAddress()
```

### LicensingModule\_\_IncompatibleRoyaltyPolicyDerivativeRevShare

```solidity
error LicensingModule__IncompatibleRoyaltyPolicyDerivativeRevShare()
```

### LicensingModule\_\_IncompatibleLicensorCommercialPolicy

```solidity
error LicensingModule__IncompatibleLicensorCommercialPolicy()
```

### LicensingModule\_\_IncompatibleLicensorRoyaltyDerivativeRevShare

```solidity
error LicensingModule__IncompatibleLicensorRoyaltyDerivativeRevShare()
```

### LicensingModule\_\_DerivativeRevShareSumExceedsMaxRNFTSupply

```solidity
error LicensingModule__DerivativeRevShareSumExceedsMaxRNFTSupply()
```

### LicensingModule\_\_MismatchBetweenRoyaltyPolicy

```solidity
error LicensingModule__MismatchBetweenRoyaltyPolicy()
```

### LicensingModule\_\_RegisterPolicyFrameworkMismatch

```solidity
error LicensingModule__RegisterPolicyFrameworkMismatch()
```

### LicensingModule\_\_RoyaltyPolicyNotWhitelisted

```solidity
error LicensingModule__RoyaltyPolicyNotWhitelisted()
```

### LicensingModule\_\_MintingFeeTokenNotWhitelisted

```solidity
error LicensingModule__MintingFeeTokenNotWhitelisted()
```

### LicensingModule\_\_DisputedIpId

```solidity
error LicensingModule__DisputedIpId()
```

emitted when trying to interact with an IP that has been disputed in the DisputeModule

### LicensingModule\_\_LinkingRevokedLicense

```solidity
error LicensingModule__LinkingRevokedLicense()
```

emitted when linking a license from a licensor that has been disputed in the DisputeModule

### LicensingModuleAware\_\_CallerNotLicensingModule

```solidity
error LicensingModuleAware__CallerNotLicensingModule()
```

### PolicyFrameworkManager\_\_GettingPolicyWrongFramework

```solidity
error PolicyFrameworkManager__GettingPolicyWrongFramework()
```

### PolicyFrameworkManager\_\_CommercializerCheckerDoesNotSupportHook

```solidity
error PolicyFrameworkManager__CommercializerCheckerDoesNotSupportHook(address commercializer)
```

### LicensorApprovalChecker\_\_Unauthorized

```solidity
error LicensorApprovalChecker__Unauthorized()
```

### DisputeModule\_\_ZeroArbitrationPolicy

```solidity
error DisputeModule__ZeroArbitrationPolicy()
```

### DisputeModule\_\_ZeroArbitrationRelayer

```solidity
error DisputeModule__ZeroArbitrationRelayer()
```

### DisputeModule\_\_ZeroDisputeTag

```solidity
error DisputeModule__ZeroDisputeTag()
```

### DisputeModule\_\_ZeroLinkToDisputeEvidence

```solidity
error DisputeModule__ZeroLinkToDisputeEvidence()
```

### DisputeModule\_\_NotWhitelistedArbitrationPolicy

```solidity
error DisputeModule__NotWhitelistedArbitrationPolicy()
```

### DisputeModule\_\_NotWhitelistedDisputeTag

```solidity
error DisputeModule__NotWhitelistedDisputeTag()
```

### DisputeModule\_\_NotWhitelistedArbitrationRelayer

```solidity
error DisputeModule__NotWhitelistedArbitrationRelayer()
```

### DisputeModule\_\_NotDisputeInitiator

```solidity
error DisputeModule__NotDisputeInitiator()
```

### DisputeModule\_\_NotInDisputeState

```solidity
error DisputeModule__NotInDisputeState()
```

### DisputeModule\_\_NotAbleToResolve

```solidity
error DisputeModule__NotAbleToResolve()
```

### DisputeModule\_\_NotRegisteredIpId

```solidity
error DisputeModule__NotRegisteredIpId()
```

### DisputeModule\_\_UnauthorizedAccess

```solidity
error DisputeModule__UnauthorizedAccess()
```

### ArbitrationPolicySP\_\_ZeroDisputeModule

```solidity
error ArbitrationPolicySP__ZeroDisputeModule()
```

### ArbitrationPolicySP\_\_ZeroPaymentToken

```solidity
error ArbitrationPolicySP__ZeroPaymentToken()
```

### ArbitrationPolicySP\_\_NotDisputeModule

```solidity
error ArbitrationPolicySP__NotDisputeModule()
```

### RoyaltyModule\_\_ZeroRoyaltyPolicy

```solidity
error RoyaltyModule__ZeroRoyaltyPolicy()
```

### RoyaltyModule\_\_NotWhitelistedRoyaltyPolicy

```solidity
error RoyaltyModule__NotWhitelistedRoyaltyPolicy()
```

### RoyaltyModule\_\_ZeroRoyaltyToken

```solidity
error RoyaltyModule__ZeroRoyaltyToken()
```

### RoyaltyModule\_\_NotWhitelistedRoyaltyToken

```solidity
error RoyaltyModule__NotWhitelistedRoyaltyToken()
```

### RoyaltyModule\_\_NoRoyaltyPolicySet

```solidity
error RoyaltyModule__NoRoyaltyPolicySet()
```

### RoyaltyModule\_\_IncompatibleRoyaltyPolicy

```solidity
error RoyaltyModule__IncompatibleRoyaltyPolicy()
```

### RoyaltyModule\_\_NotAllowedCaller

```solidity
error RoyaltyModule__NotAllowedCaller()
```

### RoyaltyModule\_\_ZeroLicensingModule

```solidity
error RoyaltyModule__ZeroLicensingModule()
```

### RoyaltyModule\_\_CanOnlyMintSelectedPolicy

```solidity
error RoyaltyModule__CanOnlyMintSelectedPolicy()
```

### RoyaltyModule\_\_NoParentsOnLinking

```solidity
error RoyaltyModule__NoParentsOnLinking()
```

### RoyaltyModule\_\_NotRegisteredIpId

```solidity
error RoyaltyModule__NotRegisteredIpId()
```

### RoyaltyPolicyLAP\_\_ZeroRoyaltyModule

```solidity
error RoyaltyPolicyLAP__ZeroRoyaltyModule()
```

### RoyaltyPolicyLAP\_\_ZeroLiquidSplitFactory

```solidity
error RoyaltyPolicyLAP__ZeroLiquidSplitFactory()
```

### RoyaltyPolicyLAP\_\_ZeroLiquidSplitMain

```solidity
error RoyaltyPolicyLAP__ZeroLiquidSplitMain()
```

### RoyaltyPolicyLAP\_\_NotRoyaltyModule

```solidity
error RoyaltyPolicyLAP__NotRoyaltyModule()
```

### RoyaltyPolicyLAP\_\_ZeroLicensingModule

```solidity
error RoyaltyPolicyLAP__ZeroLicensingModule()
```

### RoyaltyPolicyLAP\_\_AboveParentLimit

```solidity
error RoyaltyPolicyLAP__AboveParentLimit()
```

### RoyaltyPolicyLAP\_\_AboveAncestorsLimit

```solidity
error RoyaltyPolicyLAP__AboveAncestorsLimit()
```

### RoyaltyPolicyLAP\_\_AboveRoyaltyStackLimit

```solidity
error RoyaltyPolicyLAP__AboveRoyaltyStackLimit()
```

### RoyaltyPolicyLAP\_\_InvalidAncestorsLength

```solidity
error RoyaltyPolicyLAP__InvalidAncestorsLength()
```

### RoyaltyPolicyLAP\_\_InvalidAncestors

```solidity
error RoyaltyPolicyLAP__InvalidAncestors()
```

### RoyaltyPolicyLAP\_\_InvalidRoyaltyAmountLength

```solidity
error RoyaltyPolicyLAP__InvalidRoyaltyAmountLength()
```

### RoyaltyPolicyLAP\_\_InvalidAncestorsHash

```solidity
error RoyaltyPolicyLAP__InvalidAncestorsHash()
```

### RoyaltyPolicyLAP\_\_InvalidParentRoyaltiesLength

```solidity
error RoyaltyPolicyLAP__InvalidParentRoyaltiesLength()
```

### RoyaltyPolicyLAP\_\_InvalidAncestorsRoyalty

```solidity
error RoyaltyPolicyLAP__InvalidAncestorsRoyalty()
```

### RoyaltyPolicyLAP\_\_ImplementationAlreadySet

```solidity
error RoyaltyPolicyLAP__ImplementationAlreadySet()
```

### RoyaltyPolicyLAP\_\_ZeroAncestorsVaultImpl

```solidity
error RoyaltyPolicyLAP__ZeroAncestorsVaultImpl()
```

### RoyaltyPolicyLAP\_\_NotFullOwnership

```solidity
error RoyaltyPolicyLAP__NotFullOwnership()
```

### RoyaltyPolicyLAP\_\_UnlinkableToParents

```solidity
error RoyaltyPolicyLAP__UnlinkableToParents()
```

### RoyaltyPolicyLAP\_\_TransferFailed

```solidity
error RoyaltyPolicyLAP__TransferFailed()
```

### RoyaltyPolicyLAP\_\_LastPositionNotAbleToMintLicense

```solidity
error RoyaltyPolicyLAP__LastPositionNotAbleToMintLicense()
```

### AncestorsVaultLAP\_\_ZeroRoyaltyPolicyLAP

```solidity
error AncestorsVaultLAP__ZeroRoyaltyPolicyLAP()
```

### AncestorsVaultLAP\_\_AlreadyClaimed

```solidity
error AncestorsVaultLAP__AlreadyClaimed()
```

### AncestorsVaultLAP\_\_InvalidAncestorsHash

```solidity
error AncestorsVaultLAP__InvalidAncestorsHash()
```

### AncestorsVaultLAP\_\_InvalidClaimer

```solidity
error AncestorsVaultLAP__InvalidClaimer()
```

### AncestorsVaultLAP\_\_ClaimerNotAnAncestor

```solidity
error AncestorsVaultLAP__ClaimerNotAnAncestor()
```

### AncestorsVaultLAP\_\_ETHBalanceNotZero

```solidity
error AncestorsVaultLAP__ETHBalanceNotZero()
```

### AncestorsVaultLAP\_\_ERC20BalanceNotZero

```solidity
error AncestorsVaultLAP__ERC20BalanceNotZero()
```

### AncestorsVaultLAP\_\_TransferFailed

```solidity
error AncestorsVaultLAP__TransferFailed()
```

### ModuleRegistry\_\_ModuleAddressZeroAddress

```solidity
error ModuleRegistry__ModuleAddressZeroAddress()
```

### ModuleRegistry\_\_ModuleAddressNotContract

```solidity
error ModuleRegistry__ModuleAddressNotContract()
```

### ModuleRegistry\_\_ModuleAlreadyRegistered

```solidity
error ModuleRegistry__ModuleAlreadyRegistered()
```

### ModuleRegistry\_\_NameEmptyString

```solidity
error ModuleRegistry__NameEmptyString()
```

### ModuleRegistry\_\_NameAlreadyRegistered

```solidity
error ModuleRegistry__NameAlreadyRegistered()
```

### ModuleRegistry\_\_NameDoesNotMatch

```solidity
error ModuleRegistry__NameDoesNotMatch()
```

### ModuleRegistry\_\_ModuleNotRegistered

```solidity
error ModuleRegistry__ModuleNotRegistered()
```

### ModuleRegistry\_\_InterfaceIdZero

```solidity
error ModuleRegistry__InterfaceIdZero()
```

### ModuleRegistry\_\_ModuleTypeAlreadyRegistered

```solidity
error ModuleRegistry__ModuleTypeAlreadyRegistered()
```

### ModuleRegistry\_\_ModuleTypeNotRegistered

```solidity
error ModuleRegistry__ModuleTypeNotRegistered()
```

### ModuleRegistry\_\_ModuleNotSupportExpectedModuleTypeInterfaceId

```solidity
error ModuleRegistry__ModuleNotSupportExpectedModuleTypeInterfaceId()
```

### ModuleRegistry\_\_ModuleTypeEmptyString

```solidity
error ModuleRegistry__ModuleTypeEmptyString()
```

### RegistrationModule\_\_InvalidOwner

```solidity
error RegistrationModule__InvalidOwner()
```

The caller is not the owner of the root IP NFT.

### AccessController\_\_IPAccountIsZeroAddress

```solidity
error AccessController__IPAccountIsZeroAddress()
```

### AccessController\_\_IPAccountIsNotValid

```solidity
error AccessController__IPAccountIsNotValid(address ipAccount)
```

### AccessController\_\_SignerIsZeroAddress

```solidity
error AccessController__SignerIsZeroAddress()
```

### AccessController\_\_CallerIsNotIPAccount

```solidity
error AccessController__CallerIsNotIPAccount()
```

### AccessController\_\_PermissionIsNotValid

```solidity
error AccessController__PermissionIsNotValid()
```

### AccessController\_\_RecipientIsNotRegisteredModule

```solidity
error AccessController__RecipientIsNotRegisteredModule(address to)
```

### AccessController\_\_PermissionDenied

```solidity
error AccessController__PermissionDenied(address ipAccount, address signer, address to, bytes4 func)
```

### AccessControlled\_\_ZeroAddress

```solidity
error AccessControlled__ZeroAddress()
```

### AccessControlled\_\_NotIpAccount

```solidity
error AccessControlled__NotIpAccount(address ipAccount)
```

### AccessControlled\_\_CallerIsNotIpAccount

```solidity
error AccessControlled__CallerIsNotIpAccount(address caller)
```

### TaggingModule\_\_InvalidRelationTypeName

```solidity
error TaggingModule__InvalidRelationTypeName()
```

### TaggingModule\_\_RelationTypeAlreadyExists

```solidity
error TaggingModule__RelationTypeAlreadyExists()
```

### TaggingModule\_\_SrcIpIdDoesNotHaveSrcTag

```solidity
error TaggingModule__SrcIpIdDoesNotHaveSrcTag()
```

### TaggingModule\_\_DstIpIdDoesNotHaveDstTag

```solidity
error TaggingModule__DstIpIdDoesNotHaveDstTag()
```

### TaggingModule\_\_RelationTypeDoesNotExist

```solidity
error TaggingModule__RelationTypeDoesNotExist()
```
