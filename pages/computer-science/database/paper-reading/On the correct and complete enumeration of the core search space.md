- Join中的转换规则
    - 交换率：comm
        - 定义：左右孩子可以交换
        - 算子兼容性 ![image.jpg](../assets/64e00fb3-2636-4d33-a9f9-a046fa66aafe-1115003.jpg)
    - 结合率：assoc
        - 一般的结合率（结合率不是对称的）
            - 定义：改变优先级（做树的转换）<img src="https://api2.mubu.com/v3/document_image/f7757907-9a06-4b40-8e3a-f0b24cac775e-1115003.jpg" /> ![image.jpg](../assets/7b0502b8-c2b6-4c84-a172-16bc6c1c5618-1115003.jpg)
            - 兼容性 ![image.jpg](../assets/1d356946-61ed-44b2-9f3d-ec05d8803f8c-1115003.jpg)
        - 左/右结合
            - 定义
                - 左结合：改变左子树（右子树的上升下降）<img src="https://api2.mubu.com/v3/document_image/23981dab-bcad-478a-9688-1461c857b85f-1115003.jpg" /> ![image.jpg](../assets/927ce69d-29f7-4024-a19c-29a2d00c7edc-1115003.jpg)
                - 右结合：改变右子树（左子树的上升下降）<img src="https://api2.mubu.com/v3/document_image/43841ea6-50e3-4422-8522-679e62ce007d-1115003.jpg" /> ![image.jpg](../assets/651bb8aa-ac82-401b-b255-c13e51d5ba14-1115003.jpg)
            - 算子兼容性（对称） ![image.jpg](../assets/499490aa-1c08-4641-b795-9bfc6a3a964e-1115003.jpg)
- 如何利用上述三张表中的冲突：在枚举的时候进行冲突检测 ![image.jpg](../assets/24fc83f1-d700-4e6d-bf24-e084d496945b-1115003.jpg)
- 如果冲突检测：即判断当前变化是否可用
    - 一些辅助数据结构
        - SES：算子谓词引用的表，我们需要保证它全部出现在子树中 ![image.jpg](../assets/8bf1ac11-bf99-4b81-afb4-fe50438d2e64-1115003.jpg)
        - TES：全部兼容性需求的表，我们需要保证它全部出现在子树中（在CD-B/C 算法，其和SES应该等价）
        - L/RÍ-TES：必须出现在左孩子的表 ![image.jpg](../assets/7f6006bc-4247-4320-8cd8-f8804fb196f8-1115003.jpg)
    - CD-A：利用TES来禁止一些不允许的规则
        - 禁止规则
            - 对于交换率与左结合 ![image.jpg](../assets/6bc5d918-7de2-4fb5-8560-54321f376e1d-1115003.jpg)
            - 对于交换率与右结合 ![image.jpg](../assets/b67780db-8b83-4063-b317-b9c491ede335-1115003.jpg)
        - 构造TES：自底向上
            - 考虑到多个不可结合的算子不是直接孩子的情况 ![image.jpg](../assets/d3fba201-7715-4d7d-aea4-f89788550e67-1115003.jpg)
            - 所以我们需要自底向上的构造，构造算法：注意是针对所有的子算子（不仅是下一层的） ![image.jpg](../assets/b37e120b-04b6-4b67-9475-8720cc7b21f2-1115003.jpg)
            - 证明
                - 正确性
                    - 对于assoc
                - 完备性：不存在
                    - 反例 ![image.jpg](../assets/6bd211b3-392e-4594-8d19-1b8cb3c9d1ed-1115003.jpg)
                        - ​​​​​​​​​​与​​​​​​​​​​没有有结合率，所以我们是的​​​​​​​​​​​​​​​​​​​​​​​​
                        - 由此ban掉了plan1 plan2
        - 兼容性检测：查看左右孩子是否满足需求 ![image.jpg](../assets/54618a42-c8c5-421b-a637-f37b9bfc8040-1115003.jpg)
    - CD-B ：利用规则来禁止一些不允许的规则
        - 规则
            - 对于规则​​​​​​​​​​​​​​​​​​​​，我们有如下要求​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​
                - 即如果T1出现在算子​​​​​的子树中
                - 那么T2必然属于算子​​​​​
        - 对于禁止的rule
            - 交换率+左结合 ![image.jpg](../assets/42afb252-7507-40bf-a1ae-8243e3c003e4-1115003.jpg)
            - 交换率+右结合 ![image.jpg](../assets/b4453182-a84d-4caa-957b-5eeb4d9d5722-1115003.jpg)
        - 构造同样是自底向上 ![image.jpg](../assets/b67cb154-2b5e-4ec7-91b9-81d2a47b0b95-1115003.jpg)
        - 兼容性检测 ![image.jpg](../assets/ca611d94-495f-4ed7-a115-4bfb4b0d1016-1115003.jpg)
        - 证明
            - 正确性
            - 完备性
                - 反例 ![image.jpg](../assets/1b3b52d5-66fc-40a4-90ae-8fef22026e42-1115003.jpg)
                    - 由于​​​​​​​​​​​​​​​​​​​​​​无右结合率，所以​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​​
    - CD-C：进一步放松规则
        - 对于规则的放松 ![image.jpg](../assets/f0de5815-d03b-4394-8077-abe85bc9c336-1115003.jpg)
            - 考虑到谓词不包含一些情况
- 规则的一些简化 ![image.jpg](../assets/313ded8c-0054-4298-8ecc-c3d5e6ef7a8a-1115003.jpg)