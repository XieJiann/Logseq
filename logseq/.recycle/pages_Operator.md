- k8s 架构
    - 简单的分层 ![image.jpg](../assets/fd50ff75-c72a-4c4b-8dd0-7b914d5be9bc-1115003.jpg)
        - control plane
            - controller：定时监控应用的状态是否满足要求
        - application plane
    - Operator
        - 问题
            - Stateful app是很难抽象的，每个应用可能有着不同的state抽象
            - k8s只提供最简单的抽象，并提供一种扩展机制
            - 通过这种扩展，构建的app及其controller被称为operator
        - 工作机制
            - k8s 通过api 暴露 application 的一些属性
            - CR 本质是扩展 k8s api，添加新的监控属性（通过CRD，即监控的数据schema）
            - controller 通过循环调用对应的api作出反应
            - api ![image.jpg](../assets/30c3c56c-391b-4508-8c2b-140f40e64400-1115003.jpg)
            - etcd operator例子 ![image.jpg](../assets/e45aa477-33f7-49cf-91ec-2a3ac1b8125b-1115003.jpg)
