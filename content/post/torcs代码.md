+++ 
title = "torcs代码" 
date = "Wed, 06 Jan 2016 11:10:00 GMT" 
tags = ["TORCS"] 
categories = ["Machine Learning"]
description = " TORCS 的数据结构" 
+++ 

```
/** Info returned by driver during the race */
typedef struct {
    tdble   steer;      /**< Steer command [-1.0, 1.0]  */
    tdble   accelCmd;   /**< Accelerator command [0.0, 1.0] */
    tdble   brakeCmd;   /**< Brake command [0.0, 1.0] */
    tdble   clutchCmd;  /**< Clutch command [0.0, 1.0] */
    int     gear;       /**< [-1,6] for gear selection */
    int     raceCmd;    /**< command issued by the driver */
#define RM_CMD_NONE     0   /**< No race command */
#define RM_CMD_PIT_ASKED    1   /**< Race command: Pit asked */
    char    msg[4][32];     /**< 4 lines of 31 characters 0-1 from car 2-3 from race engine */
#define RM_MSG_LEN  31
    float   msgColor[4]; /**< RGBA of text */
    int     lightCmd;    /**< Lights command */
#define RM_LIGHT_HEAD1      0x00000001  /**< head light 1 */
#define RM_LIGHT_HEAD2      0x00000002  /**< head light 2 */
} tCarCtrl;

```