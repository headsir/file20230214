# 一、 根据已知2点经纬度计算方位角

```
def calc_azimuth(lat1, lon1, lat2, lon2):
    """
    根据已知2点经纬度计算方位角
    :param lat1: 纬度1
    :param lon1: 经度1
    :param lat2: 纬度2
    :param lon2: 经度2
    :return: 方位角
    """
    lat1_rad = lat1 * math.pi / 180
    lon1_rad = lon1 * math.pi / 180
    lat2_rad = lat2 * math.pi / 180
    lon2_rad = lon2 * math.pi / 180

    y = math.sin(lon2_rad - lon1_rad) * math.cos(lat2_rad)
    x = math.cos(lat1_rad) * math.sin(lat2_rad) - math.sin(lat1_rad) * math.cos(lat2_rad) * math.cos(
        lon2_rad - lon1_rad)

    brng = math.atan2(y, x) * 180 / math.pi

    return float((brng + 360.0) % 360.0)

lat1 = 113.685140
lon1 = 34.747928
lat2 = 113.704594
lon2 = 34.752756
l = calc_azimuth(lon1,lat1, lon2,  lat2)
print(l)
```

# 二、根据已知2点经纬度计算距离

VBA计算方法

```
方法：=6371004*SQRT(POWER(COS(B3*PI()/180)*(C3*PI()/180-A3*PI()/180),2)+POWER((D3*PI()/180-B3*PI()/180),2))
地球半径：6371004米
SQRT：平方根函数
POWER：数字幂函数
PI（）：圆周率
```

python 计算方法

```
from math import radians, cos, sin, asin, sqrt
# 公式计算两点经纬度之间的距离（m）
def geodistance(lng1, lat1, lng2, lat2):

    # 经纬度转换成弧度
    lng1, lat1, lng2, lat2 = map(radians, [float(lng1), float(lat1), float(lng2), float(lat2)])
    dlon = lng2 - lng1
    dlat = lat2 - lat1
    a = sin(dlat / 2) ** 2 + cos(lat1) * cos(lat2) * sin(dlon / 2) ** 2
    distance = 2 * asin(sqrt(a)) * 6371 * 1000  # 地球平均半径：6371km
    return round(distance, 3)
print(geodistance(113.685054, 34.747858, 113.697566, 34.747695))
```

调用geopy包中的方法

```
from geopy.distance import geodesic

print(geodesic((纬度1，经度1),(纬度2，经度2)).m)
print(geodesic((纬度1，经度1),(纬度2，经度2)).km)
```



