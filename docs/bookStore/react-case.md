### onChange事件 行内写法

    <RadioGroup value={this.state.isDoctor} onChange={e => { this.setState({ sortBy: e.target.value }) }}>
        <Radio value={true}>是</Radio>
        <Radio value={false}>否</Radio>
    </RadioGroup>