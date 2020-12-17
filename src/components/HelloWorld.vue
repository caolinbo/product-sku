<template>
  <div class="details-main">
    <div class="details-main-product">
      <div class="product-title">销售属性</div>
      <div class="product-main">
        <div class="sale-attr" v-if="saleAttrList.length > 0" v-for="item in saleAttrList">
          <label>{{item.label}}:</label>
          <el-checkbox v-for="info in item.attrList" @change="checkAttr(info)" v-model="info.checkedValue"
                       :key="info.id">{{info.label}}
          </el-checkbox>
        </div>
        <div style="color: red;text-align: center;padding: 10px 0;" v-if="saleAttrList.length < 1">
          该分类下暂无销售属性，请先为该分类添加销售属性。
        </div>
      </div>
    </div>
    <div class="details-main-product">
      <div class="product-title">SKU定义</div>
      <div class="product-main">
        <div>
          <ul style="border: 1px solid #f4f4f5;border-bottom: none;" v-if="skuList.length > 0">
            <li class="sku-title">
              <!--<span v-for="item in skuTitle">{{item.label}}</span>-->
              <span v-for="(item,index) in skuTitle" :key="index">{{item.pName}}</span>
            </li>
            <li class="sku-table" v-for="(item,index) in skuList" :key="index">
              <div class="input-wrap" v-for="(info,i) in item.attrList" :key="i">
                <el-input type="text" size="small" v-model="info.label" disabled placeholder="请输入..."></el-input>
              </div>
              <!--<div class="input-wrap">-->
              <!--<el-input type="text" v-model="item.label" disabled placeholder="请输入..."></el-input>-->
              <!--</div>-->
              <div class="input-wrap">
                <upload-img ref="skuImgList" :image-count="1" size="mini" class="sku-img"></upload-img>
              </div>
              <div class="input-wrap">
                <el-input type="number" size="small" v-model="item.originalPrice" placeholder="请输入..."></el-input>
              </div>
              <div class="input-wrap">
                <el-input type="number" size="small" v-model="item.price" placeholder="请输入..."></el-input>
              </div>
              <div class="input-wrap">
                <el-input type="text" size="small" v-model="item.skuNumber" placeholder="请输入..."></el-input>
              </div>
              <div class="input-wrap"><span style="color: blue;cursor: pointer;"
                                            @click="removeSku(item,index)">删除</span></div>
            </li>
          </ul>
          <div style="color: red;text-align: center;padding: 10px 0;" v-else>请选择完销售属性，再来填写sku定义。</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import UploadImg from "./uploadImg";

  export default {
    components: {
      UploadImg
    },
    data() {
      return {
        //SKU定义
        skuTitle: [
          {
            pName: '图片',
          },
          {
            pName: '原价(元)'
          },
          {
            pName: '售价(元)'
          },
          {
            pName: '商家SKU编号'
          },
          {
            pName: ''
          }
        ],
        skuList: [],
        descartesArr1: [],
        descartesArr2: [],
        //销售属性
        saleAttrList: [
          {
            label: '颜色',
            id: '1',
            attrList: [
              {
                label: '白色',
                pId: '1',
                id: '11',
                pName: '颜色',
                checkedValue: false
              },
              {
                label: '黑色',
                pId: '1',
                id: '12',
                pName: '颜色',
                checkedValue: false
              },
              {
                label: '紫色',
                pId: '1',
                id: '13',
                pName: '颜色',
                checkedValue: false
              },
              {
                label: '红色',
                pId: '1',
                id: '14',
                pName: '颜色',
                checkedValue: false
              },
              {
                label: '蓝色',
                pId: '1',
                id: '15',
                pName: '颜色',
                checkedValue: false
              }
            ]
          },
          {
            label: '尺寸',
            id: '2',
            attrList: [
              {
                label: '25',
                pId: '2',
                id: '21',
                pName: '尺寸',
                checkedValue: false
              },
              {
                label: '26',
                pId: '2',
                id: '22',
                pName: '尺寸',
                checkedValue: false
              },
              {
                label: '27',
                pId: '2',
                id: '23',
                pName: '尺寸',
                checkedValue: false
              },
              {
                label: '28',
                pId: '2',
                id: '24',
                pName: '尺寸',
                checkedValue: false
              }
            ]
          },
          {
            label: '配置',
            id: '3',
            attrList: [
              {
                label: '牛逼',
                pId: '3',
                id: '31',
                pName: '配置',
                checkedValue: false
              },
              {
                label: '很牛逼',
                pId: '3',
                id: '32',
                pName: '配置',
                checkedValue: false
              },
              {
                label: '非常牛逼',
                pId: '3',
                id: '33',
                pName: '配置',
                checkedValue: false
              }
            ]
          },
          {
            label: '测试',
            id: '4',
            attrList: [
              {
                label: '你好',
                pId: '4',
                id: '41',
                pName: '测试',
                checkedValue: false
              },
              {
                label: '我好',
                pId: '4',
                id: '42',
                pName: '测试',
                checkedValue: false
              },
              {
                label: '大家好',
                pId: '4',
                id: '43',
                pName: '测试',
                checkedValue: false
              },
              {
                label: '你不好',
                pId: '4',
                id: '44',
                pName: '测试',
                checkedValue: false
              },
              {
                label: '我不好',
                pId: '4',
                id: '45',
                pName: '测试',
                checkedValue: false
              },
              {
                label: '大家都不好',
                pId: '4',
                id: '46',
                pName: '测试',
                checkedValue: false
              },
            ]
          }
        ],
      }
    },
    methods: {
//列表中sku删除
      /**
       * 1、先删除this.skuList列表内对应的那一项
       *
       * 2、把当前删除this.skuList列表内这项的attrList内的两个（或一个）ID拿出来
       *
       * 3、判断当前列表内还有没有删除的这两个ID，如果有什么都不做。如果没有先把this.saleAttrList选中状态取消，然后再删除对应的descartesArr1（或descartesArr2）数据
       *
       * 4、如果列表内数据已经删完了，则列表title内的前两项同样删除
       * @param item 要删除的数据
       * @param index 下标
       */
      removeSku(item, index) {
        // console.log(JSON.stringify(this.descartesArr1));
        // console.log(JSON.stringify(this.descartesArr2));
        // console.log(JSON.stringify(item));
        //删除时需要删除sku列表数据
        //还要删除descartesArr1 和 descartesArr2这两个对应的数据
        //最后还要删除skuTitle的数据
        //1、先删除this.skuList列表内对应的那一项
        this.skuList.splice(index, 1);
        //第一步结束
        //2、把当前删除this.skuList列表内这项的attrList内的两个（或一个）ID拿出来
        let deleteId1 = item.attrList[0].id;
        let deleteId2 = '';
        if (item.attrList.length > 1) {
          deleteId2 = item.attrList[1].id;
        }
        //第二步结束
        //3、判断当前列表内还有没有删除的这两个ID，如果有什么都不做。如果没有先把this.saleAttrList选中状态取消，然后再删除对应的descartesArr1（或descartesArr2）数据
        let isDeleteAttrBool1 = true;
        let isDeleteAttrBool2 = true;
        this.skuList.map((info, index) => {
          info.attrList.map((data, i) => {
            if (data.id == deleteId1) isDeleteAttrBool1 = false;
            if (data.id == deleteId2) isDeleteAttrBool2 = false;
          })
        });
        if (isDeleteAttrBool1) {
          this.saleAttrList.map((data, index) => {
            item.attrList.map((info, i) => {
              if (info.id == deleteId1) {
                info.checkedValue = false;
              }
            })
          });
          let i = -1;
          this.descartesArr1.map((info, index) => {
            if (info.id == deleteId1 || info.id == deleteId2) {
              i = index;
            }
          });
          if (i != -1) {
            this.descartesArr1.splice(i, 1);
          }
        }

        if (isDeleteAttrBool2) {
          this.saleAttrList.map((data, index) => {
            item.attrList.map((info, i) => {
              if (info.id == deleteId2) {
                info.checkedValue = false;
              }
            })
          });
          let k = -1;
          this.descartesArr2.map((info, index) => {
            if (info.id == deleteId1 || info.id == deleteId2) {
              k = index;
            }
          });
          if (k != -1) {
            this.descartesArr2.splice(k, 1);
          }
        }
        //第三步结束
        //4、如果列表内数据已经删完了，则列表title内的前两项同样删除
        if (this.skuList.length < 1) {
          this.skuTitle.splice(1, 1);
          this.skuTitle.splice(0, 1);
        }

        // console.log("删除后----------------------")
        // console.log(JSON.stringify(this.descartesArr1));
        // console.log(JSON.stringify(this.descartesArr2));
        // console.log(JSON.stringify(item));

      },
      //销售属性选择
      /**
       * 1、把选择的数据抽出来分别放在数组里
       *
       * 2、如果descartesArr1 和 descartesArr2 两个数组内有数据 并且 当前选择的pId和 descartesArr1 与 descartesArr2 两个数组内的pId都不相同，那代表用户的选择的属性超过2个了
       *
       * 3、循环得到取消的属性并删除,包含sku列表title一起删除
       *
       * 4、处理数据，利用笛卡尔积算法把选择的属性数据处理一下
       *
       * 5、把笛卡尔积算法处理过的数据变为可以在页面渲染的数据
       *
       *
       * @param info 选择的数据
       * @returns {boolean}
       */
      checkAttr(info) {
        //笛卡尔积算法最终合在一起的数组
        let calcDescartesArr = [];
        // console.log("选择的数据", info);
        //判断是勾选还是取消
        if (info.checkedValue) {
          //2、如果descartesArr1 和 descartesArr2 两个数组内有数据 并且 当前选择的pId和 descartesArr1 与 descartesArr2 两个数组内的pId都不相同，那代表用户的选择的属性超过2个了
          if (this.descartesArr1.length > 0 && this.descartesArr2.length > 0) {
            if (this.descartesArr1[0].pId != info.pId && this.descartesArr2[0].pId != info.pId) {
              info.checkedValue = false;
              this.$message({
                type: 'warning',
                message: '最多选择两个属性'
              });
              return false;
            }
          }
          //第二步结束-------------

          //1、把选择的数据抽出来分别放在数组里
          if (this.descartesArr1.length < 1) {
            //第一次添加
            this.descartesArr1.unshift(info);
          } else {
            //不是第一次添加则判断pId是否和descartesArr1内的pId相同，相同则添加descartesArr1，不同则添加descartesArr2
            if (this.descartesArr1[0].pId == info.pId) {
              this.descartesArr1.unshift(info);
            } else {
              this.descartesArr2.unshift(info);
            }
          }
          //第一步结束------

          //列表title
          if (this.skuTitle.length == 5 && this.descartesArr1.length > 0) {
            this.skuTitle.unshift(this.descartesArr1[0]);
          } else if (this.skuTitle.length == 6 && this.descartesArr2.length > 0) {
            this.skuTitle.unshift(this.descartesArr2[0]);
            let sku0 = this.skuTitle[0];
            this.skuTitle[0] = this.skuTitle[1];
            this.skuTitle[1] = sku0;
          }
        } else {
          //3、循环得到取消的属性并删除,包含sku列表title一起删除
          let i = -1;
          this.descartesArr1.map((item, index) => {
            if (item.id == info.id) {
              i = index;
            }
          });
          if (i != -1) this.descartesArr1.splice(i, 1);

          let k = -1;
          this.descartesArr2.map((item, index) => {
            if (item.id == info.id) {
              k = index;
            }
          });
          if (k != -1) this.descartesArr2.splice(k, 1);

          //删除列表title
          if (this.descartesArr1.length < 1 || this.descartesArr2.length < 1) {
            let o = -1;
            this.skuTitle.map((item, index) => {
              if (item.id == info.id) {
                o = index;
              }
            });
            if (this.skuTitle.length != 6) {
              if (o != -1) {
                this.skuTitle.splice(o, 1);
              }
            } else {
              if (this.descartesArr1.length < 1 && this.descartesArr2.length < 1) {
                if (o != -1) {
                  this.skuTitle.splice(o, 1);
                }
              }
            }

          }
          //第三步结束 ----------
        }
        //4、处理数据，利用笛卡尔积算法把选择的属性数据处理一下
        let arr = [this.descartesArr1, this.descartesArr2];
        // console.log('this.descartesArr1', this.descartesArr1);
        // console.log('this.descartesArr2', this.descartesArr2);
        calcDescartesArr = this.calcDescartes(arr);
        // console.log('calcDescartesArr----', JSON.stringify(calcDescartesArr));
        //第四步结束----------

        //5、把笛卡尔积算法处理过的数据变为可以在页面渲染的数据
        if (calcDescartesArr.length > 0) {
          let listArr = [];
          if (calcDescartesArr[0].id) {
            //获取列表内数据
            if (calcDescartesArr.length > 1) {
              calcDescartesArr.map((item, index) => {
                listArr.push(
                  {
                    imgUrl: '',
                    originalPrice: '',
                    price: '',
                    skuNumber: '',
                  }
                );
                listArr[index].attrList = [];
                listArr[index].attrList.push(item);
              })
            } else {
              listArr.push(
                {
                  attrList: calcDescartesArr,
                  imgUrl: '',
                  originalPrice: '',
                  price: '',
                  skuNumber: '',
                }
              )
            }

          } else {
            //获取列表内数据
            calcDescartesArr.map((item, index) => {
              listArr.push(
                {
                  attrList: item,
                  imgUrl: '',
                  originalPrice: '',
                  price: '',
                  skuNumber: '',
                }
              )

            });
          }
          this.skuList = listArr;
        } else {
          this.skuList = [];
        }
        // console.log('this.skuList', this.skuList);
        // console.log('this.skuTitle', this.skuTitle);
      },
      //笛卡尔积算法
      calcDescartes(array) {
        if (array.length < 2 && array[1].length < 1 && array[0].length < 1) return array[0];
        if (array[1].length < 1) return array[0];
        if (array[0].length < 1) return array[1];
        return [].reduce.call(array, function (col, set) {
          let res = [];
          col.forEach(function (c) {
            set.forEach(function (s) {
              let t = [].concat(Array.isArray(c) ? c : [c]);
              t.push(s);
              res.push(t);
            })
          });
          return res;
        });
      },
    }
  }
</script>


<style scoped>

  .details-main {
    padding: 0 10px 20px 10px;
  }

  .details-main-product {
    border: 1px solid #f4f4f5;
    width: 100%;
    margin-bottom: 20px;
  }

  .product-title {
    font-size: 14px;
    font-weight: bold;
    background-color: #f4f4f5;
    padding: 10px 20px;
  }

  .product-main {
    padding: 10px 20px;
  }

  .sale-attr {
    margin-bottom: 10px;
  }

  .sku-title {
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid #f4f4f5;

  }

  .sku-title span {
    display: inline-block;
    width: 13%;
    text-align: center;
  }

  .sku-table {
    height: 80px;
    line-height: 80px;
    border-bottom: 1px solid #f4f4f5;

  }

  .input-wrap {
    display: inline-block;
    width: 13%;
    text-align: center;
  }

</style>
<style>
  .sku-img {
    display: inline;
  }

  .sku-img .el-upload--picture-card {
    vertical-align: middle;
  }

  .sku-img .el-upload-list--picture-card {
    vertical-align: text-top;
  }
</style>
