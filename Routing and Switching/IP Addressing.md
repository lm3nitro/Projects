# Basic Routing

### Scope:

In this project, I will simulate a real-world network scenario where three routers are deployed in different geographical locations (Georgia, California, and Colorado), with basic IP connectivity configured for remote communication. The goal is to establish a network topology where the routers can communicate with each other.

### Topology:

![Pasted image 20240508130031](https://github.com/lm3nitro/Projects/assets/55665256/b7a55415-4394-4500-9bc9-16cf53fc7316)

Subnet Information:

Georgia California: 10.10.10.0/30
Georgia Colorado: 10.10.20.0/30
Colorado California: 10.10.30.0/30

## Georgia:

Listing interfaces:

![Pasted image 20240508130347](https://github.com/lm3nitro/Projects/assets/55665256/2ffb230f-c26e-44ad-82ca-66ec110abffd)

Configuring the interfaces:

Link to California:

![Pasted image 20240508130455](https://github.com/lm3nitro/Projects/assets/55665256/36404c1d-9085-410c-bc58-58e5287f19f9)

Link to Colorado:

![Pasted image 20240508130723](https://github.com/lm3nitro/Projects/assets/55665256/ec7d5141-055a-4973-a085-742590bcde4a)

Checking the status of the interfaces:

![Pasted image 20240508131038](https://github.com/lm3nitro/Projects/assets/55665256/53bbdf93-0b7d-41e6-88c9-76dd27442b5d)

## California:

Listing interfaces:

![Pasted image 20240508133214](https://github.com/lm3nitro/Projects/assets/55665256/fd9cf3ba-df52-4034-8de3-b0445714f64c)

Configuring interfaces:

Link to Georgia:

![Pasted image 20240508133318](https://github.com/lm3nitro/Projects/assets/55665256/92a00d2b-cb83-4ddb-b050-cbc8b925b1ab)

Link to Colorado:

![Pasted image 20240508133359](https://github.com/lm3nitro/Projects/assets/55665256/60113dde-5bcb-47d8-b123-bd3819812f69)

Checking the status of the interfaces:

![Pasted image 20240508133420](https://github.com/lm3nitro/Projects/assets/55665256/9e165d5a-608c-4ec9-bddb-59ba59a01c68)

## Colorado:

Listing interfaces:

![Pasted image 20240508131954](https://github.com/lm3nitro/Projects/assets/55665256/b1028314-7003-43a4-a144-8ee4fa5a55a7)

Configuring the interfaces:

Link to Georgia:

![Pasted image 20240508132335](https://github.com/lm3nitro/Projects/assets/55665256/b612cff9-8f77-4bb6-b980-bc0ac0e19873)

Link to California:

![Pasted image 20240508132627](https://github.com/lm3nitro/Projects/assets/55665256/33c70522-5634-4aeb-935f-d6ac7c1eea37)

Checking the status of the interfaces:

![Pasted image 20240508132700](https://github.com/lm3nitro/Projects/assets/55665256/4b28befd-63ad-4452-b32d-a806b0661573)

## Testing Connectivity:

Georgia:

![Pasted image 20240508132803](https://github.com/lm3nitro/Projects/assets/55665256/2a29c262-3064-44bd-b529-2e5c9b1f2dea)

California:

![Pasted image 20240508133451](https://github.com/lm3nitro/Projects/assets/55665256/e2e48297-3190-47ba-9ca3-64e65b4210ec)

Colorado:

![Pasted image 20240508133536](https://github.com/lm3nitro/Projects/assets/55665256/14af2897-be54-4d61-9d4e-56de8eae1148)

### Summary:

This setup simulates a real-world network environment where three routers in different geographic locations are interconnected, enabling basic IP connectivity between the sites. In real-world scenarios, companies often have geographically dispersed offices—sometimes located in different cities, countries, or continents—each requiring secure and reliable communication between locations. 
