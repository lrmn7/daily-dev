**Last updated**: October 6, 2019


```py
def encode_label(df, return_object=False, using_class=False):
  # function to encode label to number and return new dataframe
  from sklearn.preprocessing import LabelEncoder
  temp = df.copy()
  class_list = {}
  objList = temp.select_dtypes(include = "object").columns
  for feat in objList:
    le = LabelEncoder()
    if using_class:
      le = using_class[feat]
    else:
      le = le.fit(temp[feat].astype(str))
      class_list.update({feat:le})
    temp[feat] = le.transform(temp[feat].astype(str))
  # if you wanna return le object
  if return_object:
    return temp, class_list
  return temp

# how to use
df_encoded, le_objects = encode_label(df, return_object=True)

#for example, you wanna encode your test data
#and you need pre-trained encoder from your data train
df_test_encoded, le_test_objects = encode_label(df_test, return_object=True, using_class=le_objects)
```