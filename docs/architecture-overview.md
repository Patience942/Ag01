# 非标自动化设备架构总览

下面给出一个适用于非标自动化设备的典型系统架构概览，覆盖安全、控制、执行、检测和上位管理等核心模块。

```mermaid
flowchart LR
    subgraph 上位与管理层[上位与管理层]
        MES[MES / ERP / 调度系统]
        HMI[HMI / 触摸屏]
        SCADA[监控与报警系统]
    end

    subgraph 控制层[控制层]
        PLC[PLC 控制器]
        IPC[工业PC / 运动控制器]
        SAFE[安全控制器 / 急停系统]
        NET[工业以太网 / IO 采集]
    end

    subgraph 执行层[执行层]
        AXIS[伺服/步进轴]
        CYL[气缸 / 执行器]
        VAC[真空系统]
        GRIP[夹具 / 定位机构]
        TRANS[输送 / 物料搬运]
    end

    subgraph 感知层[感知层]
        SENS[位置传感器 / 光电 / 接近]
        VISION[视觉检测 / 编码识别]
        FLOW[气压 / 真空 / 温度监测]
    end

    subgraph 机械层[机械层]
        FRAME[机架 / 导轨 / 滑块]
        BELT[皮带 / 链条 / 传动]
        TOOL[加工 / 夹持 / 组装工具]
    end

    subgraph 保障层[保障层]
        ALM[报警 / 事件记录]
        LOG[日志 / 维护记录]
        DIAG[故障诊断 / 快速排查]
    end

    MES --> HMI
    HMI --> PLC
    MES --> SCADA
    SCADA --> PLC

    PLC --> SAFE
    PLC --> IPC
    IPC --> NET
    NET --> PLC

    PLC --> AXIS
    PLC --> CYL
    PLC --> VAC
    PLC --> GRIP
    PLC --> TRANS

    AXIS --> FRAME
    CYL --> TOOL
    GRIP --> TOOL
    TRANS --> FRAME

    SENS --> PLC
    VISION --> PLC
    FLOW --> PLC

    FRAME --> SENS
    BELT --> SENS
    TOOL --> SENS

    PLC --> ALM
    ALM --> LOG
    LOG --> DIAG
    DIAG --> HMI

    SAFE -. 保护停机 .-> AXIS
    SAFE -. 保护停机 .-> CYL
    SAFE -. 保护停机 .-> TRANS
```

## 架构说明

- 上位与管理层：负责生产指令、订单下发、监控和报警联动。
- 控制层：实现逻辑控制、运动控制、安全联锁和设备状态管理。
- 执行层：包括伺服轴、气缸、夹具、真空吸盘和输送机构等设备动作部件。
- 感知层：通过传感器和视觉系统反馈位置、状态、产品缺陷和工艺参数。
- 机械层：为设备提供结构支撑、运动路径、传动和工艺承载能力。
- 保障层：用于报警处理、日志归档、故障诊断和现场快速排查。

## 典型运行流程

1. 操作员或上位系统发出生产指令。
2. PLC 根据工艺逻辑和安全状态控制各执行器动作。
3. 传感器和视觉系统持续反馈实时状态。
4. 设备完成动作后，将结果反馈给 HMI 或 MES。
5. 若出现异常，安全系统优先停机并触发报警与维护记录。

## 建议扩展

- 增加 OPC UA / MQTT 接口，便于设备数据接入工业互联网平台。
- 增加在线诊断模块，用于趋势分析、故障预测和维护提醒。
- 建立标准化 I/O 点表和安全逻辑表，提升设备调试与维护效率。

