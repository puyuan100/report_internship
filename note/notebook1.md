

# 20210629 - presentation
  Sub-wavelength periodic structures are arrangements of different materials with a pitch small enough to suppress the diffraction effects arising from their periodicity.

  However, it has been with more recent development of photonic technologies and high-resolution lithography that sub-wavelength structures have seen widespread application in integrated optics.

# the content of speach
  Hello everyone, now I'll give you a brief presentation about what I've done during the two monthes. The content of my work is simple and clear, which is to get a good performance optical coupler. And the coupler should be able to be used to coupler the light from an optical fiber to a chip. or from the chip to an optical fiber.

  At the beginning I prefer to do a little explaination of my idea. Just as what I know, we often use two different design for a coupler. One is the grating structure and one is the taper structure. here I present some of their characteristics. As we can see, the grating structure can prove highly efficiency, and higher misalignment tolerance. More importantly, the grating can redirect the light , to allow the coupler happens out of plane. And this is what I need.

  Then by reading some papers, I have a better understanding of the sub-wavelength structure. knowing its advantages. And here I only want to say its two conditions. one is the crosswise condition, which means the light propagate along the x or y direction. And the other is the lengthwise condition, the light propagate along the z axis.

  Here we firstly consider the propagation crosswise condition. Here we assume the light propagate along x direction at the figure. If we consider the diffraction effect , we can get a formula like this. Here I can express the form of sin theta. It's obvious that  when the lambda over the big lambda, which is the length of the pitch. And when this result is bigger than one, we can say for all the value of k, the value of sin theta is bigger than one. which means we now suppress the diffraction effect. The subwavelength stucture can work as a waveguide.

  And for the propagation lengthwise condition, we can also get a formula to describe the diffraction effect. I worte it here.
  

# 20210702
## first
  I firstly set
  
  ``` matlab
    num_Lsi = 9;
    num_Lgap = 9;
    num_neff = 7;

    Lsi = linspace(0.21,0.45,num_Lsi)*1e-9;
    Lgap = linspace(0.21,0.45,num_Lgap)*1e-9;
    n_eff = [2.2, 2.3, 2.4, 2.5, 2.6, 2.7, 2.8];

    N = linspace(0,0,num_N)

    % in my code I use matrix "note_CE", "note_OL", "note_AN" to restore the CE, OL and Angle with the change of Lsi and Lgap. So for each matrix different column represents the different Lgap; and different row represents different Lsi 
  ```

| target      | n   | Lsi(um) | Lgap(um) | max value        |
| -           | -   | -       | -        | -                |
| maximize CE | 2.4 | 0.3     | 0.36     | max_CE = 00.4796 |
| maximize OL | 2.4 | 0.27    | 0.36     | max_OL = 0.8356  |

  I found the maximum value is taken at the boundary of Lgap. Therefore, I decide to boarden the range of the Lgap. And run the simulation again.
  
  ``` matlab
    num_Lsi = 9;
    num_Lgap = 3;
    num_neff = 7;

    Lsi = linspace(0.21,0.45,num_Lsi)*1e-9;
    Lgap = [0.48, 0.51, 0.54]*1e-9;
    n_eff = [2.2, 2.3, 2.4, 2.5, 2.6, 2.7, 2.8];

    N = linspace(0,0,num_N)
  ```

  I found the data I got above is correct. And this data (Lgap = [0.48, 0.51, 0.54]*1e-9) is not necessary.

## second
   After finding the best n, Lsi, Lgap to obtain maximum CE and OL. The I adopt the Apodization.
   I firstly let N = [2 3 4 5 6 7] (for Apodized), which means I change the Lsi and Lgap at the first N periods of the structure. Then I note the value of n, Lsi, Lgap for each N to get the maximum CE.

   | target | N | n   | L_si(um) | L_gap(um) | Max_CE |
   | -      | - | -   | -        | -         | -      |
   | max CE | 2 | 2.7 | 0.27     | 0.36      | 0.5141 |
   | max CE | 3 | 2.7 | 0.3      | 0.33      | 0.5201 |
   | max CE | 4 | 2.7 | 0.3      | 0.33      | 0.5253 |
   | max CE | 5 | 2.8 | 0.3      | 0.3       | 0.5309 |
   | max CE | 6 | 2.8 | 0.3      | 0.3       | 0.5287 |
   | max CE | 7 | 2.8 | 0.3      | 0.3       | 0.5310 |

   Then for each group of n, L_si, L_gap I set N = linspace(1,30,30), to see the change of CE.

# 20210722
  我发现了一个问题。当使用 linear apodization 时，不仅要线性改变（前一部分周期）有效折射率，还应该随时调整 Lsi 和 Lgap 的数值。其调整方法基于如下考量：
  之前早已得出在统一的条件下，neff = 2.4时可以得到最佳的偶和效率。值得注意的是此时藕合角度为 7.5 度。因此对于 linear apodization 部分，任意一小段（对应一个折射率）都应该具有相同的藕合角度。（通过调整Lsi,Lgap实现）

| Target Angle | n    | Cloest Angle | Lsi  | Lgap | Lsi2 | Lgap2 |
| -            | -    | -            | -    | -    | -    | -     |
| 7.5          | 2.4  | 7.5          | 0.39 | 0.24 | 0.3  | 0.36  |
| 7.5          | 2.45 | 6.9          | 0.27 | 0.39 | x    | x     |
| 7.5          | 2.5  | 8.2          | 0.3  | 0.21 | x    | x     |
| 7.5          | 2.55 | 7.1          | 0.24 | 0.42 | x    | x     |
| 7.5          | 2.6  | 7.2          | 0.39 | 0.21 | x    | x     |
| 7.5          | 2.65 | 7.6          | 0.21 | 0.45 | x    | x     |
| 7.5          | 2.7  | 7            | 0.24 | 0.39 | x    | x     |
| 7.5          | 2.75 | 8            | 0.33 | 0.27 | x    | x     |
| 7.5          | 2.8  | 8.1          | 0.3  | 0.3  | 0.21 | 0.42  |
