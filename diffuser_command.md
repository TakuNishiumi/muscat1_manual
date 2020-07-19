# muscat1_manual

## diffuser
  python diffuser.py
### 1. Usage
Please run `diffuser -h` in shine.  

##### 6 digits meaning
Status of diffusers is expressed 6 digits of `1` and `0`. `1` expresses 'on(diffuser inserted)', `o` expresses 'off(not inserted)'.
For example, If status is `001001`.  

| | | 
|:--:|:--:|
|diffuser|`ABABAB`|
|CCD number|`001122`|  
|6 digits |`001001`|

This means diffuser A of ccd=1 and diffuserB of ccd=2 are on and the others are off.  
  

`diffuser --on=0A`  
  This means diffuser A of ccd=0 will only insert. Others will be off (not inserted). This returns `100000`  
`diffuser --diffA`  
  This means diffuser A of all ccd will insert. Others will be off.  This returns `101010`  
`diffuser --diffB`  
  This means diffuser B of all ccd will insert. Others will be off.  This returns `010101`  
`diffuser --manual=001001`  
  This means diffuser A of ccd=1 and diffuser B of ccd=2 will only insert. Others will be off. This returns `001001`.   
`diffuser --off`  
  This means all diffusers will be off.  




### 2. Explanation
This command is on-off switches of diffusers.  
You can insert two diffusers in each band. We calls this diffuser A and diffuser B. Diffuser A is strong scattering and diffuser B is weak scattering. Of course, you can also use diffuser A + diffuser B as very strong scattering diffuser.  


### 3. Known issues 
1. After rebooting alex, It returns unknown status at the first time. 
  - Please ignore this.
1. This commands stacked sometimes.
  - Please rebooting alex.
    - If it didn't repair after rebooting alex, please run `screen /dev/ttyS0`, it shows new terminal. Please input `PCA01`
    - If it returns `OK`, please quit the new terminal and search this process(`ps aux`) and kill this.
    




## suggest_exptime-focus
### 1. Usage  
  ```
  usage: suggest_exptime-focus.py [-h] -c CCD [-f F0] [-n NEXP] [-s STEP]
                                  [-p PEAK] [-r RAD] [-no]

  This command suggests BEST exptime and focus value.

  optional arguments:
    -h, --help            show this help message and exit
    -c CCD, --ccd CCD     CCD for find_focus and CCD  for calculating SNR. (ex. : --ccd=2)
    -f F0, --f0 F0        f0 value in find_focus(ex. --f0=70500), default=70200
    -n NEXP, --nexp NEXP  Number of exposure in find_focus (ex. --nexp=5), default=7
    -s STEP, --step STEP  Step of find_focus (ex. --step=100), default=150
    -p PEAK, --peak PEAK  Targets' peak count (ADU) (ex. --peak=40000), default=35000
    -r RAD, --rad RAD     Aperture radius of finding stars (pixel) (ex. --rad=20), default=30
    -no, --no_find_focus  Use previous find_focus result. (ex. : --no_find_focus)

  (ex1. suggests --ccd=2 
  (ex2. suggests --ccd=2 --f0=70500 --nexp=5 --step=100 --peak=40000 
  (ex3. suggests -c=2 -f=70500 -n=5 -s=100 -p=40000 
  (ex4. suggests --ccd=2 --peak=40000 -no
  usage: suggest_exptime-focus.py [-h] -c CCD [-f F0] [-n NEXP] [-s STEP]
                                  [-p PEAK] [-r RAD] [-no]
  ```  
    
### 2. Explanation of the command  
  This command suggests BEST exposure times and focus value automatically. 
  1. Execute `find_focus` command at first (if it is without "-no" option), and find best focus.
  1. Stars are shown with numbers.
  1. Select number of the target, and Input it. 
  1. Calculates peak of the target.
  1. Calculates suitable focus value by result of `find_focus`.
  1. Tests exposure with the suggested exptimes and focus value.
  1. Suggests Best exptimes and focus value.

### 3. Warning  
  1. Some cameras were broken, when this command was tested. So this was modified to One-color-version at present. If you would like to apply this command to three camera, you have to run `suggest_exptime-focus.py` three times. (Second(-c=1) and third(-c=2) are recommended to use "-no" option.)
  1. (suggested exptimes > 40s) will not be tested.
  1. Defocus is limited by (FWHM < 35 pixel) and distance between target and other stars.   
  
  Taku will modify this command to multi-color-version and with-diffusers-version in the future.  
  If you have any questions, please send me an e-mail.
