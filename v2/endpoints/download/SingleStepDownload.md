Single Step Download
-------------------------------------
The single step download call returns a redirect to download the largest file available for the specified image (ImageId). 
This endpoint currently does not support ImagePack or RoyaltyFreeSubscription ProductOfferings and only supports downloading images at this time. Single step download encapsulates the functionality of [GetLargestImageDownloadAuthorizations](GetLargestImageDownloadAuthorizations.md), [CreateDownloadRequest](CreateDownloadRequest.md) and the call to download an image into a single request.  

###Download Limits
Most ProductOfferings have enforced periodic download limits such as monthly, 
weekly, and daily. When this operation executes, the count of allowed downloads is 
decremented by one. Once the download limit is reached, no further downloads 
may be requested until the next download period.

The download limit for a given download period is covered in your license 
agreement established with Getty Images. Please contact your Getty Images account manager for more information.


###Endpoint
Use the following endpoint (HTTPS only) to access this operation:

  https://connect.gettyimages.com/v2/download

###Request
  POST https://connect.gettyimages.com/v2/download/{ImageId}

####HTTP Headers
| Name               | Value              | Use      | 
|:-------------------|:-------------------|:---------|
| Authorization      | Bearer AccessToken | Required |
| Api-Key            | ApiKey             | Required |
| GI-Coordination-Id | CoordinationId     | Optional |

###Response
| HTTP Status   | Type    | Description                                                |
|:--------------|:--------|:-----------------------------------------------------------|
| 302           | Success | Redirects to download URI                                  |
| 401           | Error   | Only a secure token can be used with this operation        |
| 401           | Error   | This token is not fully authenticated                      |
| 403           | Error   | A secure token cannot be passed over a non-SSL/TLS channel |
| 403           | Error   | Product offering can not be used with this resource        |
| 403           | Error   | No product offering found                                  |
| 404           | Error   | Image not found                                            |
| 404           | Error   | No authorizations were found for the asset                 |

####HTTP Headers
| Name               | Value              |  
|:-------------------|:-------------------|
| Location           | download URI       |
| GI-Coordination-Id | CoordinationId     |

####Sample Request
    POST https://connect.gettyimages.com/v2/download/177670965 HTTP/1.1
    Host: connect.gettyimages.com
    Authorization: Bearer {access_token}
    Api-Key: {api_key}
    
####Sample Response
    HTTP/1.1 302 Found
    Location: {Redirect URI}

####Sample Response for Redirect
    HTTP/1.1 200 OK
    Date: Tue, 20 May 2014 20:55:32 GMT
    Server: Microsoft-IIS/6.0
    X-Powered-By: ASP.NET
    X-AspNet-Version: 2.0.50727
    Content-Disposition: attachment; filename=177670965.jpg
    Content-Length: 16214107
    Accept-Ranges: bytes
    Cache-Control: private
    Last-Modified: Sat, 24 Aug 2013 00:26:39 GMT
    Content-Type: application/x-download


###Workflow Example
1. Call [OAuth2](../oauth2) to get an access token.
2. Call [SearchForImages](../search/SearchForImages.md) to find images.
3. Call Download to download the image.

####Notes
A 403 "No product offering found" will also be returned if your ProductOffering has a quota, and has been exhausted.
