<template>
    <div class="layout">
        <Layout>
            <Header :style="{position: 'fixed', width: '100%'}">
                <Menu mode="horizontal" theme="dark" active-name="1">
                    <div class="layout-nav">
                        <MenuItem name="1">
                            <Icon type="ios-paper"></Icon>
                            日志查询平台
                        </MenuItem>
                    </div>
                </Menu>
            </Header>
            <Content :style="{margin: '88px 20px 0', background: '#fff', minHeight: '500px'}">
            <br>
                <div id="list">
                        <div>
                            <Row>
                                <Col span="2">
                                    <div align="right">
                                        <label>DeviceId:</label>
                                    </div>
                                </Col>

                                <Col span="3">
                                    <div>
                                        <Input v-model="logform.deviceId" name="deviceId" size="small"/>
                                    </div>
                                </Col>

                                <Col span="2">
                                    <div align="right">
                                        <label>APP渠道:</label>
                                    </div>
                                </Col>

                                <Col span="3">
                                    <div >
                                        <Input v-model="logform.channel" name="channel" size="small"/>
                                    </div>
                                </Col>
                            </Row>

                            <Row>

                                <Col span="2">
                                    <div align="right">
                                         <label align="right">开始时间:</label>
                                    </div>
                                </Col>

                                <Col span="3">
                                    <div >
                                        <Input  v-model="logform.fromDate" name="fromDate" size="small"/>
                                    </div>
                                </Col>

                                <Col span="2">
                                    <div align="right">
                                        <label>结束时间:</label>
                                    </div>
                                </Col>

                                <Col span="3">
                                    <div>
                                        <Input v-model="logform.toDate" name="toDate" size="small"/>
                                    </div>
                                </Col>

                                <Col span="2">
                                    <div align="right">
                                        <Button @click="search" type="primary" :size="buttonSize">日志查询</Button>
                                    </div>
                                </Col>
                            </Row>
                        </div>
                        <Divider dashed />
                        <div>
                            <Table stripe :columns="columns" :data="historyData"></Table>
                            <Page :total="dataCount" :page-size="pageSize"  @on-change="changepage" transfer=true></Page>
                        </div>
                </div>
            </Content>
            <Footer class="layout-footer-center">2011-2016 &copy; TalkingData</Footer>
        </Layout>
    </div>
</template>



<script>
import Layout from 'iview/src/components/layout'
import Header from 'iview/src/components/header'
import Menu from 'iview/src/components/menu'
import MenuItem from 'iview/src/components/menu-item'
import Icon from 'iview/src/components/icon'
import Content from 'iview/src/components/content'
import Col from 'iview/src/components/col'
import Row from 'iview/src/components/row'
import Footer from 'iview/src/components/footer'
import Divider from 'iview/src/components/divider'
import Button from 'iview/src/components/button'
import Input from 'iview/src/components/input'
import Page from 'iview/src/components/page'

export default {
    name: 'List',
    components:{
                    'Col': Col,
                    'Row': Row,
                    'MenuItem': MenuItem,
                    'Icon': Icon,
                    'Divider': Divider,
                    'Layout': Layout,
                    'Button': Button,
                    'Input': Input,
                    'Header': Header,
                    'Menu': Menu,
                    'Content': Content,
                    'Footer': Footer,
                    'Page': Page,
                },
    data () {
        return {
            buttonSize: 'small',
            columns: [
                {
                    type: 'index',
                    width: 60,
                    align: 'center'
                },
                {
                    title: '设备号',
                    width: 120,
                    key: 'deviceId'
                },
                {
                    title: '渠道',
                    width: 120,
                    key: 'channel'
                },
                {
                    title: '日志信息',
                    key: 'logInfo'
                }
                          ],
            logform: {},
            dataCount: 0,
            pageSize: 10,
            ajaxHistoryData: [],
            historyData: [],
            num: 1,
          }
    },

    methods: {
        search () {
            var self = this
            var param = self.logform
            param['size']=self.pageSize
            param['page']= 1
            this.$http.post('http://localhost:8081/ymm-info-log/query',param).then(function (response){
            self.dataCount = response.data.result.totalCount
            self.historyData = response.data.result.rows
            }).catch(function (error){
              console.log(error)
            })
        },

        changepage(index){
            var self = this
            var param = self.logform
            param['size']=self.pageSize
            param['page']= index
            this.$http.post('http://localhost:8081/ymm-info-log/query',param).then(function (response){
            self.historyData = response.data.result.rows
            }).catch(function (error){
              console.log(error)
            })
        }, 
    }
}
</script>


<style scoped>
.layout{
        border: 1px solid #d7dde4;
        background: #f5f7f9;
        position: relative;
        border-radius: 4px;
        overflow: hidden;
    }
    .layout-nav{
        width: 440px;
        margin: 0 auto;
        margin-left: 20px;
    }
    .layout-footer-center{
        text-align: center;
    }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

}
</style>
