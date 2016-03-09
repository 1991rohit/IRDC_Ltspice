from numpy import *
from scipy import *
import matplotlib.pyplot as plt
T1=20e-3;Ro=50e3;Rx=75e3;kx_ideal=(Rx-Ro)/Ro
data=open(r'C:\Users\Rohit\Google Drive\Improved RDC study\4. LTspice\freq sweep(kx=0.5)(Cc=5pF)\freq=10.0Hz to 100Hz.txt','r')
level_initial=1;level_final=1;N1=0.0;N2=0.0;L1=[];L2=[];fint=[];kx_meas=[];err=[];step=-1;
for i, line in enumerate(data):
    line=line.strip()
    line=line.split()
    if (line[0]=='Step'):
        L1.append(N1);
        L2.append(N2);
        kx=0;N1=0.0;N2=0.0;step+=1;
        F=line[2].replace('F=','');
        f=float(F)
        fint.append(f)
    if(not((line[0] == "time")or(line[0] == "Step"))):
        level_initial=level_final
        if(float(line[0])<T1):
            if(float(line[1])<2.5):
                level_final=0;
            else:
                level_final=1;
            if(level_initial != level_final):
                N1+=1
            #level.append(level_final)
        else:
            if(float(line[1])<2.5):
                level_final=0;
            else:
                level_final=1;
            if(level_initial!=level_final):
                N2+=1
L1.append(N1)
L2.append(N2);
del L1[0]
del L2[0]
for i in range(len(L1)):
    kx=(L2[i]-L1[i])/L1[i];
    kx_meas.append(kx)
    err.append(100.0*(kx-kx_ideal))
print L1,L2
plt.plot(fint,err,'o')
plt.grid()
plt.title('Test for Interference rejection of IRDC(at kx=0.5)')
plt.xlabel('Interfering signal frequency, fint')
plt.ylabel('% error in reading (kx_meas-kx_ideal)*100')
plt.show();
    
