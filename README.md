# UV-Anim-GTASA2.11.311-
fixed gta sa  version 2.11.311 support UV-Anim

'''
void CPatches::ApplyUVAnimPatch()
{

    // RpMaterialUVAnimApplyUpdate (RpUVAnimParamKeyFrameApply branch)
    CMemory::Write32(g_libGame + 0x732818, 0x2D428AC0); // ldp s0, s2, [x22, #0x14] (was [x22, #0x10])
    CMemory::Write32(g_libGame + 0x732820, 0xBD401EC1); // ldr s1, [x22, #0x1C]      (was [x22, #0x18])
    CMemory::Write32(g_libGame + 0x732834, 0xBD4022C0); // ldr s0, [x22, #0x20]      (was [x22, #0x1C])
    CMemory::Write32(g_libGame + 0x732840, 0xBD4026C0); // ldr s0, [x22, #0x24]      (was [x22, #0x20])
    CMemory::Write32(g_libGame + 0x73287C, 0xBD4012C0); // ldr s0, [x22, #0x10]      (was [x22, #0xC])  uv rotation angle

    // RpUVAnimParamKeyFrameApply
    CMemory::Write32(g_libGame + 0x731878, 0xBD401420); // ldr s0, [x1,  #0x14]      (was [x1,  #0x10])
    CMemory::Write32(g_libGame + 0x731888, 0xBD401C20); // ldr s0, [x1,  #0x1C]      (was [x1,  #0x18])
    CMemory::Write32(g_libGame + 0x731898, 0xBD401820); // ldr s0, [x1,  #0x18]      (was [x1,  #0x14])
    CMemory::Write32(g_libGame + 0x7318AC, 0xBD402020); // ldr s0, [x1,  #0x20]      (was [x1,  #0x1C])
    CMemory::Write32(g_libGame + 0x7318B4, 0xBD402420); // ldr s0, [x1,  #0x24]      (was [x1,  #0x20])
    CMemory::Write32(g_libGame + 0x7318DC, 0xBD401280); // ldr s0, [x20, #0x10]      (was [x20, #0xC])  uv rotation angle

    // The "360 UVAnim hack" flag (RunUVAnim, .bss @ 0xD13498) is only set while
    // RtAnimInterpolatorSetCurrentAnim runs at load time (Set360UVAnimHack(1)/(0)
    // scoped toggle in _rpUVAnimCustomDataStreamRead / RpMaterialSetUVAnim) and
    // stays 0 during gameplay. Because of that RtAnimInterpolatorAddAnimTime
    // skips the interpolation block entirely (if (RunUVAnim == 1)), so the
    // current keyframe is never re-interpolated and UV anims stay frozen.
    // NOP both flag checks so the anim system behaves like the PC version:

    // RtAnimInterpolatorAddAnimTime @ libGame.so + 0x787730:
    CMemory::Write32(g_libGame + 0x7877DC, 0xD503201F); // nop (was b.ne 0x787898 = "if (RunUVAnim == 1)")

    // RtAnimInterpolatorSetCurrentAnim @ libGame.so + 0x787400:
    CMemory::Write32(g_libGame + 0x7874C0, 0xD503201F); // nop (was tbz w10, #0 = "if (RunUVAnim & 1)")
}
'''
