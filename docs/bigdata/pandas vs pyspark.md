# pandas vs pyspark

### 使用pandas遇到的问题
#### apply方法
apply的axis参数,默认是0，即按列进行，为1时，按行
如何要使用apply处理数据时，要用到df内的数据时,使用df['xxx'] = df.apply(lambda x: test(x), axis=1)按行处理。