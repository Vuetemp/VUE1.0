<template>
    <div class="pd20 border-top">
        <g-table :config.sync="tableData" :selectvalue.sync="selectvalue" :searchvalue.sync="searchvalue">
            <div slot="search">
                <!--调用通用查询的头，方便后续修改样式-->
                <g-table-search>
                    <g-form v-ref:dictionary_searchform slot="search-content">
                        <div class="ilb pd5">
                            <g-input name="title" :label="i18n.title"></g-input>
                        </div>
                        <div class="ilb pd5">
                            <g-input name="noticeNum" :label="i18n.noticeNum"></g-input>
                        </div>
                        <div class="ilb pd5">
                            <g-select name="mvnoCode" :_list="select_mvnoCode" :label="i18n.mvno"></g-select>
                        </div>
                        <div class="ilb pd5">
                            <g-select name="status " :_list="select_statustype" :label="i18n.state"></g-select>
                        </div>
                        <div class="ilb pd5">
                            <g-select name="isRead" :_list="select_isReadtype" :label="i18n.isRead"></g-select>
                        </div>
                        <div class="ilb pd5">
                            <g-select name="emergencyDegree" :_list="select_emergencyDegreetype" :label="i18n.emergencyDegree"></g-select>
                        </div>
                        <div class="ilb pd5">
                            <g-input name="bgTime" :label="i18n.bgTime"></g-input>
                        </div>
                        <div class="ilb pd5">
                            <g-input name="endTime" :label="i18n.endTime"></g-input>
                        </div>
                        <div class="ilb pd5"></div>
                        <div class="ilb pd5"></div>
                        <div class="ilb pd5"></div>
                        <div class="ilb text-center">
                            <button type="button" class="btn btn-primary" @click.stop='e_search'
                                    v-text="i18n.search"></button>
                        </div>
                    </g-form>
                </g-table-search>
            </div>
            <button class="btn btn-primary" type="button" slot="add" @click="e_openAdd">
                <span>+</span>
                <span v-text="i18n.add"></span>
            </button>
            <div slot="btnGroup">
                <a href="javascript:" v-on:click="e_openDetail" v-text="i18n.detail"></a>
                <a href="javascript:" v-on:click="e_openEdit" v-text="i18n.edit"></a>
                <a href="javascript:" v-on:click="e_openDel" v-text="i18n.del"></a>
                <div class="a-hover">
                    <span class="iconfont icon-gengduo"></span>
                    <div class="a-hover-target">
                        <a href="javascript:" class="a-normal">发布</a>
                        <a href="javascript:" class="a-normal">下线</a>
                        <a href="javascript:" class="a-normal">浏览</a>
                    </div>
                </div>
            </div>
        </g-table>
    </div>
    <!--调用表格详情组件，将详情展示出来-->
    <g-table-detail :data.sync="selectvalue" :open.sync="openDetail"></g-table-detail>
    <!--调用表格编辑操作组件-->
    <g-dialog v-ref:dictionary_dialog_edit size="3">
        <span slot="title" v-text="i18n.edit"></span>
        <div class="clear" slot="content">
            <g-form v-ref:dictionary_editform>
                <g-input-noempty name="id" parentclass="hide"></g-input-noempty>
                <g-input-noempty name='title' :label="i18n.title" parentclass="col-xs-6"></g-input-noempty>
                <g-input-noempty name='noticeNum' :label="i18n.noticeNum" parentclass="col-xs-6"></g-input-noempty>
                <g-textarea name='description' parentclass="col-xs-12"></g-textarea>
                <div class="col-xs-6">
                    <g-select name="emergencyDegree" :_list="select_emergencyDegreetype" :label="i18n.emergencyDegree"></g-select>
                </div>
                <div class="col-xs-6">
                    <g-select name="noticeNum" :_list="select_pusWaytype" :label="i18n.noticeNum"></g-select>
                </div>
                <g-label name="createName" :label="i18n.createName" classes="col-xs-6"></g-label>
                <g-label name="createUser" :label="i18n.createUser" classes="col-xs-6"></g-label>
                <g-label name="createTime" :label="i18n.bgTime" classes="col-xs-6"></g-label>
                <g-label name="updateTime" :label="i18n.endTime" classes="col-xs-6"></g-label>
                <g-label name="mvnoCode" :label="i18n.mvno" classes="col-xs-6"></g-label>

            </g-form>
        </div>
        <button type="button" slot="button" class="btn btn-primary" @click.stop='e_editSubmit'
                v-text="i18n.edit"></button>
    </g-dialog>
    <!--调用表格新增操作组件-->
    <g-dialog v-ref:dictionary_dialog_add size="3">
        <span slot="title" v-text="i18n.add"></span>
        <div class="clear" slot="content">
            <g-form v-ref:dictionary_addform>
                <g-input-noempty name='title' :label="i18n.title" parentclass="col-xs-6"></g-input-noempty>
                <g-input-noempty name='noticeNum' :label="i18n.noticeNum" parentclass="col-xs-6"></g-input-noempty>
                <g-textarea name='description' parentclass="col-xs-12"></g-textarea>
                <!--<g-file></g-file>-->
                <div class="col-xs-6 box">
                    <g-select name="emergencyDegree" :_list="select_emergencyDegreetype" :label="i18n.emergencyDegree"></g-select>
                </div>
                <div class="col-xs-6 box">
                    <g-select name="noticeNum" :_list="select_pusWaytype" :label="i18n.noticeNum"></g-select>
                </div>
                <g-input-noempty name="createName" :label="i18n.createName" parentclass="col-xs-6 box"></g-input-noempty>
                <g-input-noempty name="createUser" :label="i18n.createUser" parentclass="col-xs-6 box"></g-input-noempty>
                <g-input-noempty name="createTime" :label="i18n.bgTime" parentclass="col-xs-6 box"></g-input-noempty>
                <g-input-noempty name="updateTime" :label="i18n.endTime" parentclass="col-xs-6 box"></g-input-noempty>
                <g-input-noempty name="mvnoCode" :label="i18n.mvno" parentclass="col-xs-6 box"></g-input-noempty>
            </g-form>
        </div>
        <button type="button" slot="button" class="btn btn-primary" @click.stop='e_addSubmit'
                v-text="i18n.add"></button>
    </g-dialog>
</template>
<script>
    import dialog from '../../../components/dialog/dialog.vue'
    import lan from '../../../libs/base/lanLoader'
    import noticeBiz from '../../biz/oms/systemManager/noticeBiz'
    import { util } from '../../../libs/base/util'
    export default{
        components: {
            gTable: require('../../../components/table/table-base.vue'),
            gForm: require('../../../components/form/form.vue'),
            gTableSearch: require('../../../components/table/table-search.vue'),
            gSelect: require('../../../components/select/select-base.vue'),
            gInput: require('../../../components/input/input-base.vue'),
            gTableDetail: require('../../../components/table/table-detail.vue'),
            gInputNoempty: require('../../../components/input/input-noempty.vue'),
            gInputReadonly: require('../../../components/input/input-readonly.vue'),
            gRadioList: require('../../../components/radio/input-radio-list.vue'),
            gTextarea: require('../../../components/textarea/textarea-desc.vue'),
            gLabel: require('../../../components/label/label-base.vue'),
//            gFile: require('../../../components/file/file.vue'),
            gDialog: dialog
        },
        methods: {
            /**
             * Created by xiaodi on 2016/12/08.
             * 查询赔偿管理——表格更新查询问题 固定套路
             */
            e_search() {
                var check = this.$refs.dictionary_searchform.getData();
                if (check.ischeck) {
                    this.searchvalue = check.data;
                }
            },
            /**
             * Created by xiaodi on 2016/12/08.
             * 打开详情弹窗 固定套路
             */
            e_openDetail() {
                this.openDetail++;
            },
            /**
             * Created by xiaodi on 2016/12/08.
             * 打开编辑弹窗 固定套路给value赋值
             */
            e_openEdit() {
                this.$refs.dictionary_dialog_edit.$emit('openDialog');
                var that = this, form = this.$refs.dictionary_editform;
                util.wait(function () {
                    var val = that.selectvalue[0].primaryValue;
                    val = util.resetFormValue({}, val);
                    form.setData(val);
                });
            },
            /**
             * Created by chenwenxiao on 2016/12/06.
             * 编辑字典表——提交编辑功能
             */
            e_editSubmit() {
                var form = this.$refs.dictionary_editform, that = this;
                var check = form.getData();
                if (check.ischeck) {
                    noticeBiz.editNotice(check.data).then(function (d) {
                        that.$refs.dictionary_dialog_edit.$emit('closeDialog');
                        //重置默认参数
                        var resetData = util.resetFormValue(check.data, {});
                        form.setData(resetData);
                        //编辑完成以后显示提示信息
                        util.middleTipsShow({
                            ico: 2,
                            txt: that.i18n.editSuccess
                        });
                    }, function (d) {
                        that.$refs.dictionary_dialog_edit.$emit('closeDialog');
                        //编辑失败以后显示提示信息
                        util.middleTipsShow({
                            ico: 1,
                            txt: that.i18n.editFailed,
                            //编辑失败以后显示提示信息后重新弹出弹出框
                            closeCallBack: function () {
                                that.$refs.dictionary_dialog_edit.$emit('openDialog');
                            }
                        });
                    });
                }
            },
            /**
             * Created by xiaodi on 2016/12/08.
             * 打开删除弹窗并处理删除操作
             */
            e_openDel() {
                var that = this;
                util.wait(function () {
                    var id = that.selectvalue[0].primaryValue.id;
                    util.dialogTipsShow({
                        txt: that.i18n.delConfirm,
                        callBack: function () {
                            noticeBiz.delNotice({
                                id: id
                            }).then(function () {
                                //删除成功以后显示提示信息
                                util.middleTipsShow({
                                    ico: 2,
                                    txt: that.i18n.delSuccess
                                });
                            }, function () {
                                //删除失败以后显示提示信息
                                util.middleTipsShow({
                                    ico: 1,
                                    txt: that.i18n.delFailed
                                });
                            });
                        }
                    })
                });
            },
            /**
             * Created by xiaodi on 2016/12/08.
             * 打开新增弹窗
             */
            e_openAdd() {
                this.$refs.dictionary_dialog_add.$emit('openDialog');
            },
            /**
             * Created by xiaodi on 2016/12/08.
             * 新增字典表——提交新增功能
             */
            e_addSubmit() {
                var form = this.$refs.dictionary_addform, that = this;
                var check = form.getData();
                if (check.ischeck) {
                    noticeBiz.addNotice(check.data).then(function (d) {
                        that.$refs.dictionary_dialog_add.$emit('closeDialog');
                        //重置默认参数
                        var resetData = util.resetFormValue(check.data, {});
                        form.setData(resetData);
                        //新增完成以后显示提示信息
                        util.middleTipsShow({
                            ico: 2,
                            txt: that.i18n.addSuccess
                        });
                    }, function (d) {
                        that.$refs.dictionary_dialog_add.$emit('closeDialog');
                        //新增失败以后显示提示信息
                        util.middleTipsShow({
                            ico: 1,
                            txt: that.i18n.addFailed,
                            //新增失败以后显示提示信息后重新弹出弹出框
                            closeCallBack: function () {
                                that.$refs.dictionary_dialog_add.$emit('openDialog');
                            }
                        });
                    });
                }
            }
        },
        data () {
            return {
                tableData: {
                    ajaxOption: {
                        url: '/oms/notice/queryNoticeByDevOps',
                        method: 'post',
                        pageSize: 5,
                        perPage: 10,
                        currPage: 1,
                        json: true
                    },
                    parton: ['noticeNum', 'title', 'content', 'emergencyDegree', 'pushWay', 'roleCodeList', 'status', 'createUser', 'createTime', 'updateTime', 'isRead'],
                    lanKey: 'v_notice',
                    timeFormat: ['createTime', 'updateTime'],
                    showSort: false,
                    showSelect: false,
                    multiSelect: false,
                    customerCol: true,
                    key: [],
                    staticKey: []
                },
                selectvalue: {},
                searchvalue: {},
                openDetail: 0,
                //国际化双向绑定对象
                i18n: {},
                //查询mvnoCdeo双向绑定
                select_mvnoCode: {},
                select_isReadtype: {},
                select_statustype: {},
                select_pusWaytype: {},
                select_emergencyDegreetype: {},
                select_lantype_edit: {
                    'zh-CN': 'zh-CN',
                    'zh-TW': 'zh-TW',
                    'en-US': 'en-US'
                },
                pushWay_tpye: {
                    '0': '普通',
                    '1': '强制'
                }
            }
        },
        ready() {
            var that = this;
            function selectMvnoCode() {
                var data = {
                    'type': 'MvnoCode',
                    'langType': 'zh-CN'
                };
                noticeBiz.searchDictionary(data).then(function (d) {
                    var mvnoCode = {};
                    $.each(d.data.dataList, function (k, v) {
                        mvnoCode[v.key] = v.value;
                        that.select_mvnoCode = mvnoCode;
                    })
                })
            }
            lan.getLan('v_notice').then(function (lanObj) {
                that.i18n = lanObj;
                that.select_isReadtype = lanObj.isRead_type;
                that.select_statustype = lanObj.status_type;
                that.select_pusWaytype = lanObj.pushWay_tpye;
                that.select_emergencyDegreetype = lanObj.emergencyDegree_type;
                selectMvnoCode();
            });
            window.$$routeList.notice = this;
        }
    }
</script>