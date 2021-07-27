1. First step is to open `msconfig.exe`:

![image](https://user-images.githubusercontent.com/48615614/127127386-3755b784-6748-49ca-b5c8-f7da0690ef8e.png)

2. Go to `Boot` tab and enable the safe boot:

![1](https://user-images.githubusercontent.com/48615614/127128118-bfc1051f-d6b3-4960-a163-d2d22506aec5.png)

3. Exit and restart the machine

![image](https://user-images.githubusercontent.com/48615614/127129842-b728dad1-7aa5-424d-8b1d-1d709db493b6.png)

4. Now open `Registry Editor` and navigate to `[HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\SepMasterService]`

![image](https://user-images.githubusercontent.com/48615614/127132175-05b76dc2-f81b-4c11-8f56-866d24b34e07.png)

5. Change the attribute `start` value from `2` to `0, 3 and 4`.

![image](https://user-images.githubusercontent.com/48615614/127133649-38c8fe98-aea8-4ea4-b910-786731be7a86.png)

Registry values
Each value stands for the following:
2 = Automatic
3 = Manual
4 = Disabled

6. Now open again the `msconfig.exe` and disable the safe boot then restart the machine:

![image](https://user-images.githubusercontent.com/48615614/127133718-807d3a29-6aac-4ccd-8247-25e9fcb5b1d2.png)
