# Configure AWS VPC Peering Connection

This project demonstrates how to configure a VPC peering connection between two VPCs in AWS, modify route tables to allow communication between the VPCs, and perform a ping test to verify connectivity.

## Prerequisites

- Two existing VPCs:
  - **VPC A (default-vpc):** 172.31.0.0/16 (AWS Default VPC)
  - **VPC B (custom-vpc):** 10.0.0.0/16 (AWS Custom VPC)
- CIDR blocks must not overlap.

## EC2 Instances

- **default-ec2:** Launched in VPC A (default-vpc) in the public subnet.
  - Attach a security group (`default-sg`) with the following rules:
    - Open SSH port (22).
    - Open All ICMP - IPv4 port.
- **custom-ec2:** Launched in VPC B (custom-vpc) in the public subnet.
  - Attach a security group (`custom-sg`) with the following rules:
    - Open SSH port (22).
    - Open All ICMP - IPv4 port.
      
![image](https://github.com/user-attachments/assets/d9de5492-e8da-453e-9377-ab155f9da81c)

## Steps

### Step 1: Create VPC Peering Connection

1. **Create Peering Connection:**
   - Navigate to the VPC Dashboard in the AWS Management Console.
   - Select "Peering Connections" from the left-hand menu.
   - Click "Create Peering Connection".
   - **Requester VPC:** Select VPC B (custom-vpc).
   - **Accepter VPC:** Select VPC A (default-vpc).
   - Confirm the peering connection request.

2. **Accept Peering Connection:**
   - In the VPC Dashboard, select "Peering Connections".
   - Find the newly created peering connection.
   - Click "Actions" and select "Accept Request".

### Step 2: Modify Route Tables

1. **VPC A (default-vpc):**
   - Navigate to the Route Tables section within the VPC Dashboard.
   - Select the route table associated with VPC A.
   - Edit the route table to add a route:
     - **Destination:** 10.0.0.0/16 (CIDR block of custom-vpc).
     - **Target:** Peering connection ID.

2. **VPC B (custom-vpc):**
   - Navigate to the Route Tables section within the VPC Dashboard.
   - Select the route table associated with VPC B.
   - Edit the route table to add a route:
     - **Destination:** 172.31.0.0/16 (CIDR block of default-vpc).
     - **Target:** Peering connection ID.

### Step 3: Ping Test

1. **Connect to EC2 Instances:**
   - Use SSH to connect to `default-ec2` in VPC A and `custom-ec2` in VPC B.

2. **Ping Test:**
   - From `custom-ec2` (in VPC B), attempt to ping the private IP address of `default-ec2` (in VPC A):
     ```sh
     ping <private-ip-of-default-ec2>
     ```
   - From `default-ec2` (in VPC A), attempt to ping the private IP address of `custom-ec2` (in VPC B):
     ```sh
     ping <private-ip-of-custom-ec2>
     ```

## Conclusion

By following these steps, you can successfully configure a VPC peering connection between two VPCs, modify route tables to allow communication between the VPCs, and verify connectivity through ping tests. This setup enables secure and efficient communication between instances in different VPCs.

## Notes

- Ensure that the security groups attached to the EC2 instances allow ICMP (ping) traffic.
- Verify that the peering connection status is "Active" before performing the ping test.
