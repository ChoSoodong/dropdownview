# dropdownview
下拉菜单
#pragma mark - DropMenu

-(void)initDropMenu
{
    // 指定默认选中
    _currentData1Index = 1;
    _currentData1SelectedIndex = 1;
    
    NSArray *food = @[@"全部美食", @"火锅", @"川菜", @"西餐", @"自助餐"];
    NSArray *travel = @[@"全部旅游", @"周边游", @"景点门票", @"国内游", @"境外游"];
    
    _data1 = [NSMutableArray arrayWithObjects:@{@"title":@"美食", @"data":food}, @{@"title":@"旅游", @"data":travel}, nil];
    _data2 = [NSMutableArray arrayWithObjects:@"全城", @"500m", @"1km", @"3km", @"5km", nil];
    _data3 = [NSMutableArray arrayWithObjects:@"智能排序", @"离我最近", @"评价最高", @"最新发布", @"人气最高", @"价格最低", @"价格最高", nil];
    _data4 = [NSMutableArray arrayWithObjects:@"不限人数", @"单人餐", @"双人餐", @"3~4人餐", nil];
    
    menu = [[JSDropDownMenu alloc] initWithOrigin:CGPointMake(0, 0) andHeight:40];
    menu.indicatorColor = [UIColor colorWithRed:175.0f/255.0f green:175.0f/255.0f blue:175.0f/255.0f alpha:1.0];
    menu.separatorColor = [UIColor colorWithRed:0.93 green:0.93 blue:0.93 alpha:0.9];//中间线
    menu.textColor = [UIColor colorWithRed:83.f/255.0f green:83.f/255.0f blue:83.f/255.0f alpha:1.0f];
    menu.dataSource = self;
    menu.delegate = self;
    
    [self.view addSubview:menu];
}

- (NSInteger)numberOfColumnsInMenu:(JSDropDownMenu *)menu//数量
{
    return 4;
}

//格式
-(BOOL)displayByCollectionViewInColumn:(NSInteger)column
{
    
    if (column==3)
    {
        
        return YES;
    }
    
    return NO;
}

-(BOOL)haveRightTableViewInColumn:(NSInteger)column
{
    
    if (column==0)
    {
        return YES;
    }
    return NO;
}

-(CGFloat)widthRatioOfLeftColumn:(NSInteger)column
{
    
    if (column==0)
    {
        return 0.3;
    }
    
    return 1;
}

-(NSInteger)currentLeftSelectedRow:(NSInteger)column
{
    
    if (column==0)
    {
        
        return _currentData1Index;
        
    }
    if (column==1)
    {
        
        return _currentData2Index;
    }
    if (column==2)
    {
        
        return _currentData3Index;
    }
    if (column==3)
    {
        
        return _currentData4Index;
    }
    return 0;
}

- (NSInteger)menu:(JSDropDownMenu *)menu numberOfRowsInColumn:(NSInteger)column leftOrRight:(NSInteger)leftOrRight leftRow:(NSInteger)leftRow
{
    if (column==0)
    {
        if (leftOrRight==0)
        {
            return _data1.count;
        } else
        {
            NSDictionary *menuDic = [_data1 objectAtIndex:leftRow];
            return [[menuDic objectForKey:@"data"] count];
        }
    } else if (column==1)
    {
        
        return _data2.count;
        
    } else if (column==2)
    {
        
        return _data3.count;
    } else if (column==3)
    {
        
        return _data4.count;
    }
    
    return 0;
}

- (NSString *)menu:(JSDropDownMenu *)menu titleForColumn:(NSInteger)column
{
    
    switch (column)
    {
        case 0: return [[_data1[_currentData1Index] objectForKey:@"data"] objectAtIndex:_currentData1SelectedIndex];
            break;
        case 1: return _data2[_currentData2Index];
            break;
        case 2: return _data3[_currentData3Index];
            break;
        case 3: return _data4[_currentData4Index];
            break;
        default:
            return nil;
            break;
    }
    
}

- (NSString *)menu:(JSDropDownMenu *)menu titleForRowAtIndexPath:(JSIndexPath *)indexPath//产生
{
    if (indexPath.column==0)
    {
        if (indexPath.leftOrRight==0)
        {
            NSDictionary *menuDic = [_data1 objectAtIndex:indexPath.row];
            return [menuDic objectForKey:@"title"];
        } else
        {
            NSInteger leftRow = indexPath.leftRow;
            NSDictionary *menuDic = [_data1 objectAtIndex:leftRow];
            return [[menuDic objectForKey:@"data"] objectAtIndex:indexPath.row];
        }
    } else if (indexPath.column==1)
    {
        return _data2[indexPath.row];
        
    } else if (indexPath.column==2)
    {
        
        return _data3[indexPath.row];
        
    }else
    {
        return _data4[indexPath.row];
    }
}

//此出可以和其他一起使用
- (void)menu:(JSDropDownMenu *)menu didSelectRowAtIndexPath:(JSIndexPath *)indexPath//点击
{
    
    if (indexPath.column == 0)
    {
        if(indexPath.leftOrRight==0)
        {
            _currentData1Index = indexPath.row;
            return;
        }
        NSInteger leftRow = indexPath.leftRow;
        NSDictionary *menuDic = [_data1 objectAtIndex:leftRow];
        NSLog(@"%@",[[menuDic objectForKey:@"data"] objectAtIndex:indexPath.row]);
        
    } else if(indexPath.column == 1)
    {
        _currentData2Index = indexPath.row;
        NSLog(@"%@",_data2[_currentData2Index]);
        
    } else if(indexPath.column == 2)
    {
        _currentData3Index = indexPath.row;
        NSLog(@"%@",_data3[_currentData3Index]);
        
    } else
    {
        _currentData4Index = indexPath.row;
        NSLog(@"%@",_data4[_currentData4Index]);
    }
    
}

