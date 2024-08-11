# 📄 UE Asset Prefixer
Simple plugin that you can edit unreal blueprint asset's prefix.

# 📄 How to Use
1. Install this plugin in the `plugins` folder.
   
2. Select blueprint assets you want to add prefix.
   
3. Right click → `Scripted Asset Actions` → `Add Prefix`  
![howtouse0](https://github.com/user-attachments/assets/1a21700c-515e-42e2-a07f-fa02ee348cf7)

4. Done!  
![howtouse1](https://github.com/user-attachments/assets/b92a6531-bd5f-47bc-8ae4-410178e2df01)

# 📄 How to Edit Prefixes
You can customize prefix mapping esaily.

See `/AssetPrefixer/Python/BP_AddPrefix`

# 🚀 All Source Code

```python
import unreal

# @ https://dev.epicgames.com/documentation/ko-kr/unreal-engine/recommended-asset-naming-conventions-in-unreal-engine-projects?application_version=4.27
prefix_mapping = {
    "Blueprint"         : "BP_",
    "Material"          : "M_",
    "MaterialInstanceConstant": "MI_",
    "Materiafuntion"    : "MF_",
    "NiagaraSystem"     : "NS_",
    "NiagaraScript"     : "NSC_",
    "NiagaraEmitter"    : "NE_",
    "Texture2D"         : "T_",
    "StaticMesh"        : "SM_",
    "SkeletalMesh"      : "SK_",
    "WidgetBlueprint"   : "WBP_",
    "ActorComponent"    : "AC_",
    "AnimationBlueprint": "ABP_",
    "BlueprintInterface": "BI_",
    "CurveTable"        : "CT_",
    "DataTable"         : "DT_",
    "Enum"              : "E_",
    "Structure"         : "F_",
    "Montages"          : "AM_",
    "AnimationSequence" : "AS_",
    "BlendSpace"        : "BS_",
    "LevelSequence"     : "LS_",
}

# Get a list of selected assets in Content Browser
selected_assets = unreal.EditorUtilityLibrary.get_selected_assets()

for asset in selected_assets:
    asset_name = asset.get_name()
    asset_class = asset.get_class()
    asset_path = unreal.Paths.get_path(asset.get_path_name())
    class_name = unreal.SystemLibrary.get_class_display_name(asset_class)

    # Check if this asset's class name has a prefix in the mapping
    class_prefix = prefix_mapping.get(class_name, None)
    if class_prefix:
        if not unreal.StringLibrary.starts_with(asset_name, class_prefix, unreal.SearchCase.CASE_SENSITIVE):
            new_name = class_prefix + asset_name
            try:
                unreal.EditorUtilityLibrary.rename_asset(asset, new_name)
                unreal.log("Add prefix success: " + asset_path + asset_name)
            except Exception as e:
                unreal.log_error(f"Could not add prefix: {e}")
    else:
        unreal.log("Prefix not found: " + class_name)
```
