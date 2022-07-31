#写在之前
项目中用到的函数简介：https://github.com/EveandBob/Introduction-to-some-functions-in-elliptic-curves-not-projects-

# 项目名称
ECDSA的实现

#实验过程
ECDSA的算法描述如下：
![Screenshot 2022-07-31 124054](https://user-images.githubusercontent.com/104854836/182010409-61ba4380-af03-454c-9358-3c9bbdb3a200.jpg)

# 部分代码
```python
def ECDSA():
    PA=[0,0]
    def ECDSA_sign(msg):
        nonlocal  PA
        M = msg.encode()
        dA, PA = get_key()
        k = random.randrange(1, n)
        R = calculate_np(Gx, Gy, k, a, b, p)
        r = R[0] % n
        e = int(sha1(M).hexdigest(), 16)
        s = (get_inverse(k, n) * (e + dA * r)) % n
        print("获得的签名为：")
        print(r,s)
        return r, s

    def ECDSA_verif_sign(msg, sign):
        r, s = sign
        M = msg.encode()
        e = int(sha1(M).hexdigest(), 16)
        w = get_inverse(s, n)
        temp1 = calculate_np(Gx, Gy, e * w, a, b, p)
        temp2 = calculate_np(PA[0], PA[1], r * w, a, b, p)
        R, S = calculate_p_q(temp1[0], temp1[1], temp2[0], temp2[1], a, b, p)
        if (r == R):
            print("结果正确")
            return True
        else:
            print("结果错误")
            return False
    return ECDSA_verif_sign(msg,ECDSA_sign(msg))
```

#结果如下
![Screenshot 2022-07-31 124521](https://user-images.githubusercontent.com/104854836/182010536-ab0af53a-4c4c-49d5-b2ee-c20315e7e41b.jpg)
