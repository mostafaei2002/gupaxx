
**Draft**

# Algorithm of distribution of Hashrate between P2pool and XvB

## Objective

If the Hashrate (HR) is not enough to probably always have at least one share in the window PPLNS (WP), the HR will never be redirected to XvB node but always stay on p2pool node.

If no share is acquired, all HR will stay on p2pool node until there is one.  

If HR is enough to probably always have at least one share in the (WP), the spare HR will be:  
**Default mode**: in part given to XvB node to be in the most possible round type and keep in p2pool the rest of HR that will not impact the type of round (sHR for spared HR).  
**Hero mode**: entirely given to the XvB node regardless of sHR.

## How

PPLNS window size (PWS): API P2pool pplnsWindowSize [^1]  
p2pool difficulty(PD): API P2pool sidechainDifficulty [^1]  
mHR: minimum required HR to stay in WP = PD / (PWS*10) [^2]  

Because it is probabilities, a buffer will be put in place for the required HR for p2pool and mHR for round type to allow some margin of errors.  
The mHR needs to be refreshed periodically because it can change with the difficulty changing. (PWS should not change).

Calculation is made in % of time that will go to p2pool and to XvB, depending if mining on mini or main side chain.  
Every ten minutes, the algorithm will decide how next 10 minutes will be distributed depending on default or hero mode.

## Manage with outside HashRate

Gupaxx could be connected to a xmrig proxy to control HR of every miners outside local machine.

## Examples

PWS = 2160  
PD = 85.5M  

### Example 1: the poor

Miner has 2kH/s on Gupaxx.  
HR never goes on XvB, because the minimum required to have a share in WP is 4kH/s based on PD and PWS values.

### Example 2: the modest

Miner has 10kH/s on Gupaxx  
for ten minutes, 4 are required to be put on p2pool.

**Default mode**: 9 minutes are given to p2pool and one for XvB.  
Because after giving 4mn to p2pool to meet mHR, he still have ~5kH/s to spare.  
The first round type (Donor round) need 1kH/s and second round type (VIP Donor) need 10kH/s.  
5kH/s is enough for the Donor Round but not enough for the VIP Donor.  
So 1kH/s is given to XvB node so that the miner participate in the Donor round.  

**Hero mode**: 4 minutes are given to p2pool and 5 for XvB.



[^1]: https://p2pool.io/mini/api/pool/stats 
[^2]: https://github.com/SChernykh/p2pool?tab=readme-ov-file#how-payouts-work-in-p2pool