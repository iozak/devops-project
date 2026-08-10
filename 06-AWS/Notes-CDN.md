**CloudFront**
Amazon CloudFront is AWS's Content Delivery Network (CDN) service that improves application performance by caching content at Edge Locations (Points of Presence - PoPs) around the world.

Instead of every user request going back to the origin server (such as an S3 bucket, EC2 instance, or Load Balancer), CloudFront serves cached content from the location closest to the user.

How CloudFront Works
1. Content is stored at an origin (e.g., S3, EC2, ALB).
2. CloudFront caches the content at Edge Locations worldwide.
3. Users request content from the nearest edge location.
4. Cached content is delivered quickly with lower latency.
5. If the content isn't cached, CloudFront retrieves it from the origin and caches it for future requests.

**CloudFront - Origins**
Origins are the source locations from which CloudFront retrieves content before delivering it to users through its global edge network.

|Origin Type|Examples|
|---|---|
|**S3 Origin**|S3 bucket (typically secured with OAC)|
|**Custom Origin**|ALB, EC2, custom web server, S3 website endpoint|

**CloudFront at a high level**
```
User
  ↓
CloudFront Edge Location
  ↓
Is content cached?
  ├─ Yes → Serve from cache → User
  │
  └─ No
       ↓
     Origin (S3/EC2/ALB/Web Server)
       ↓
   Return content
       ↓
 Cache at Edge Location
       ↓
      User
```
- Cache Hit: Content is already at the Edge Location and served immediately.
- Cache Miss: CloudFront fetches content from the origin, caches it, and then serves it.

**CloudFront with ALB or EC2 as an Origin**
CloudFront can use an Application Load Balancer (ALB) or EC2 instance as its origin instead of S3. This is commonly used for dynamic web applications.

How It Works
1. A user sends a request.
2. The request goes to the nearest CloudFront Edge Location.
3. If needed, CloudFront forwards the request to the origin:
	- ALB (which then routes traffic to backend servers)
	- EC2 instance directly
4. The response is returned through CloudFront to the user.

ALB
```
Users
  ↓
CloudFront
  ↓
Public ALB
  ↓
Private EC2 Instances
```

EC2
```
Users
  ↓
CloudFront
  ↓
Public EC2 Instance
```

