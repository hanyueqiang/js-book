### onChange事件 行内写法

    <RadioGroup value={this.state.isDoctor} onChange={e => { this.setState({ sortBy: e.target.value }) }}>
        <Radio value={true}>是</Radio>
        <Radio value={false}>否</Radio>
    </RadioGroup>


    
### React 与antd table行内事件 阻止冒泡

问题：点击`tr` 显示当前行详细信息，点击操作栏编辑删除等操作，会触发两个事件 一个是编辑函数 紧接着执行点击行内显示详细信息函数

解决： 此问题原因 事件冒泡 阻止事件冒泡即可

    <TableComponent   // 封装的Table通用组件 集成了分页 列筛选、滚动条、加载等功能
        onChange={this.onChange}
        pagination={pagination}
        dataSource={dataSource}
        loading={loading}
        rowKey={rowKey}
        columns={roleColumns(this)}
        onRow={this.onRowHandle}  //点击行 触发事件 详情看antd/Table
    />
    //行内点击触发函数 
    onRowHandle = (record) => {
        return {
            // 点击行
            onClick: () => {
                console.log(record)
            }
        };
    }
    //编辑事件 阻止冒泡 
    edit = (record, e) => {
        //阻止冒泡
        e.stopPropagation();
    }

### select下拉框 支持前端进行排序 

    <Select
        showSearch
        allowClear
        placeholder="请选择用户角色"
        filterOption={(input, option) => option.props.children.toLowerCase().indexOf(input.toLowerCase()) >= 0} 
    >
        {roleList.map((item, index) => (<Option key={index} value={item.roleId}>{item.roleName}</Option>))}
    </Select>

### antd 表单提交 加正则限制
    <FormItem
        {...formItemLayout}
            label="登录账号">
        {getFieldDecorator('userName', {
            rules: [
                {
                    required: true, message: '登录账号不能为空',
                    whitespace: false,
                },
                { max: 18, message: '登录账号不能超过18位' },
                {
                    //支持数字字母下划线 ^[A-Za-z0-9]+$
                    pattern: new RegExp('^[0-9a-zA-Z]{1,}$', 'g'), //数字字母组合
                    message: '登录账号书写格式错误'
                }
            ],
        })(
            <Input placeholder="请输入用户账号" style={{ width: 180 }} />
        )}
    </FormItem>


