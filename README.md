# Multilayer methodology for Congestion estimation 
# Data preparation
  1. Get the data only for working days
  2. Trim the time to be between 7-9 am
  3. Calculate standard deviation of the link using all the data

# Direct Layer
  Direct layer: find the travel time of the link directly from BT data extraction

# Backward layer
    If sample size of u/s link and u/s+target link are not equal to zero
      Time at target link= floor (travel time of upstream link)+time at UPS 
      Target link TT at Time at target link =TT of u/s+target link – TT of u/s link
    Else
      No data for backward layer
    End
  
# Forward layer 
    Find 𝑋(𝑡) that minimize 𝐹
    Time = t
    Optimize 𝑋(𝑡)
    Find TT 𝑍(𝑡)
    Get TT data from direct layer for link X
    Calculate the standard error of the direct layer  𝜎_(𝑥 ̅,𝑡)=𝜎_𝑡/𝑛
    For i=1:5
     Random error 𝑋(𝑡) = select random number between 0 to 𝜎_(𝑥 ̅,𝑡)
      Departure time for 𝑌 , 𝑡_𝑦= floor(𝑋(𝑡) + Random error 𝑋(𝑡) )
      Get 𝑌(𝑡_𝑦) 
      Random error 𝑍(𝑡) = select random number between 0 to 𝜎_(𝑧,𝑡)
      Random error 𝑌(𝑡_𝑦) = select random number between 0 to 𝜎_(y,𝑡_𝑦 )
      𝐹_𝑖=𝑎𝑏𝑠((𝑋(𝑡)+"Random error " 𝑋(𝑡))+(𝑌(𝑡_𝑦 )+"Random error " 𝑌(𝑡_𝑦 "))−(" 𝑍(𝑡)"+ Random error " 𝑍(𝑡)" )"
      𝐹_sto(i)= 𝐹_𝑖
    End
    𝐹=𝑠𝑢𝑚(𝐹"_sto") 
  
# Fuse the Layers
    Standard error  calculation for backward and forward layer

    𝜎_(𝑥 ̅,𝐿,𝑡)=(max⁡(𝜎_(𝐿1,𝑡,) 𝜎_(𝐿2,𝑡)))/√(min⁡(𝑛_1,𝑛_2))
    Standard error for direct layer 
    𝜎_(𝑥 ̅,𝑡)=𝜎_𝑡/𝑛
    Weight for each layer 
    𝑤_𝑖=〖𝛼𝑒〗^(−𝛼𝜎_(𝑥 ̅,𝐿,𝑡) )
    Fused TT: 
    (𝑇𝑇) ̂(𝑠,𝑠+1,𝑡)=∑1_𝑖^𝐼▒(〖(𝑇𝑇) ̂(𝑠,𝑠+1,𝑡)〗_𝑖 𝑤_𝑖)/(∑▒𝑤_𝑖 )

